# Item 8 : Avoid finalizers and cleaners

### Finalizers/cleaners 는 예측하기 어렵다
* Java 9 에서 finalizers 는 deprecated 됬고, Cleaners 는 finalizers 보다는 덜 위험하지만 역시 예측하기 어렵다
    * Finalizers, cleaners 는 객체가 unreachable 하게 될 때 즉시 invoke 된다는 보장이 없다
    * 따라서 내부적으로 time-critical 한 작업을 수행하면 안된다
* finalizers, cleaners 는 JVM 구현에 따라 다르게 동작하므로 예측하기 힘들다

### Finalizers/cleaners 는 위험하다
* finalizers 는 경우에 따라 **아예 invoke 되지 않을수도 있다**
    * 이를 해결하기 위해 `System.runFinalizersOnExit` 이나 `Runtime.runFinalizersOnExit` 을 사용하려고 해선 **안된다.** 
    모두 치명적인 결함으로 deprecated 됬다.
* finalization 을 수행할 때 발생한 uncaught exception 은 무시되기 때문에 다른 객체의 상태를 corrupt 할 수 있다
   * 일반적으로 thread 에서 uncaught exception 이 발생하면 thread 가 종료되야 하지만 **finalizers 에서 발생한 경우 종료되지 않는다**

### Finalizers/cleaners 는 심각한 performance 저하를 일으킨다
* finalizers 를 사용하면 객체 생성-파괴가 많게는 50배 이상 느려지는데, 이는 finalizers 가 효율적인 garbage collection 을 억제하기 때문이다.
* cleaners 는 일반적인 사용(모든 instance 를 clean)으로는 finalizers 와 성능이 동일하지만, *safety net* 으로만 사용한다면 
cleaners 를 사용하지 않았을 때 보다 5배 정도만 느려진다.

### Finalizers 는 심각한 보안 문제를 가지고 있다
1. constructor 혹은 이것의 serialization equivalents 인 `readObject`, `readResolve` 에서 예외가 던져진다면 
malicious subclass 의 finalizer 는 partially constructed 객체 위에서 동작할 수 있다
2. 이 finalizer 는 static field 의 object 에 대한 reference 를 record 할 수 있고, 이는 garbage collecting 을 막아버린다
3. malformed object 가 record 됬다면 **권한이 없음에도 불구하고** 임의의 method 를 invoke 할 수 있게 된다. (*finalizer attacks*)
4. **Non-final class 가 finalizer attcak 을 방어하려면 final** `finalize` **method 를 선언하고 아무 내용도 넣지 말아야 한다**

<br/>

### Termination 이 요구되는 자원을 다루는 class 는 finalizers/cleaners 대신 `AutoCloseable` 을 사용하자
* class 가 thread 나 file 같은 자원을 다룬다면 `AutoCloseable` 를 구현하고 client 가 `close()` method 를 invoke 하도록 하자
    * try-with-resources 구문을 사용하면 자동적으로 `close()` 호출
    * `close()` 호출 후에는 해당 객체가 더이상 유효하지 않다는 것을 기록해야 하고, 다른 method 에서 객체에 접근시 항상 validity 검사를 해야 한다

### 그럼에도 Finalizers/cleaners 를 사용하는 경우
* client 가 `close()` 를 호출하지 않는 경우를 대비해서 여러 단점들을 감수하고 finalizer, cleaner 내에서 자원을 해제한다. (*safety net*)
    * `FileInputStream`, `FileOutputStream`, `ThreadPoolExecutor`, `java.sql.Connection` 등이 safety net 을 적용하고 있다
* 일반적인 객체가 native(non-java) methods 를 통해 자신을 위임한 객체인 *native peer* 는 garbage collecting 대상이 아니므로 
cleaner 를 사용해서 회수 할 수 있다
    * 다만 이 경우에도 `close()` 를 선호하는 것이 좋다

### Cleaner 를 사용하는 방법
```java
   // An autocloseable class using a cleaner as a safety net
   public class Room implements AutoCloseable {
       private static final Cleaner cleaner = Cleaner.create();
       
       // Resource that requires cleaning. Must not refer to Room!
       private static class State implements Runnable {
           int numJunkPiles;  // Number of junk piles in this room
           
           State(int numJunkPiles) {
               this.numJunkPiles = numJunkPiles;
           }
           
           // Invoked by close method or cleaner
           @Override public void run() {
               System.out.println("Cleaning room");
               numJunkPiles = 0;
           }
       }
       
       // The state of this room, shared with our cleanable
       private final State state;
       
       // Our cleanable. Cleans the room when it's eligible for gc
       private final Cleaner.Cleanable cleanable;
       
       public Room(int numJunkPiles) {
           state = new State(numJunkPiles);
           cleanable = cleaner.register(this, state);
       }
       
       @Override public void close() {
           cleanable.clean();
       }
   }
```
* 위 예시에서 clean 대상은 `numJunkPiles` 변수다. numJunkPiels 가 clean 되는 경우는 아래의 2가지다
    1. client 가 `Room` 의 `close()` method 를 호출하여 `State`의 `run()` 호출
    2. `close()` 호출에 실패하더라도 cleaner 가 `run()` 호출
    
* `State` instance 가 `Room` instance 에 대한 참조가 없는 것에 주목하자. 만약에 서로가 서로를 참조한다면 **circularity** 가 발생해서 
garbage collecting 이 되지 않으며, clean 도 되지 않는다.
    * 따라서 위와 같이 nested class 를 구성하고 cleaner 를 적용하려면 **반드시 static nested class 를 사용해서 
    enclosing class 에 대한 reference 를 갖지 말아야 한다**([Item 24]())

* 다시 말하지만 cleaner 는 safety net 을 위한 것이고, `Room` class 를 cleaner 대신 try-with-resource 와 `close()` 를 사용하는 것이 좋다


***


### 요약
Safety net 용도 이외에는 **cleaner/finalizer 를 사용하지 말고,** 그런 용도로 사용한다고 해도 성능과 불확실성에 대해 유념하자.

