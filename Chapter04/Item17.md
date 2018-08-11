# Item 17 : Minimize mutability

### immutable class 를 작성하는 법
* object 의 상태를 변경하는 method(*mutators*) 를 제공하지 말아라

* class 를 상속 불가능 하게 만들어라
    * class 를 final 로 선언하거나 private constructor 만을 제공하라 

* 모든 fields 를 final 로 선언하라
    * newly created instance 의 reference 가 **synchronization 없이 다른 thread 로 전달 될때 correct behavior 를 보장한다**
    *(memory model)*

* 모든 fields 를 private 로 선언하라
    * immutable class 는 public field 를 가져도 무관하지만, client 가 field 에 직접 접근 하는 경우 추후에 내부 구현을 수정하기 힘들어지기 때문에
    private field 만을 사용하도록 하자.

* 모든 mutable components 에 대한 접근을 제한하라
    * 추가적으로 client-provided object 로 mutable field 를 초기화해서는 안된다. constructors, accessors, `readObject` 에 
    defensive copy 를 하자. ([Item 50](), [Item 88]())
    
### immutable class 의 장점
* object instance 가 불변하므로 예측하기 쉽다
    * **thread-safe 하므로 synchronization 이 필요없다.** 
    
* **instance 를 공유해서 사용**해도 전혀 지장이 없으며, 따라서 **복사가 필요없다**

* immutable instance 는 서로 **mutable field** 를 공유할 수 있다
    * `BigInteger` class 는 부호는 int 로, 크기는 int array 로 구현을 한다. 여기서 `BigInteger.negate` 는 크기는 같지만 부호만 다른 새로운
    instance 를 반환하는데, 이때 서로 **int array 를 공유한다.** 이것이 가능한 이유는 외부에서 상태 변화를 일으킬 수 없기 때문이다.

* failure atomicity 를 보장한다 ([Item 76]())

### immutable class 의 단점
* value 가 하나라도 다르다면 새로운 instance 가 필요하므로 다소 비효율적이다.
    * 특히나 instance 생성 비용이 큰 경우 문제가 되는데, 보다 효율적으로 instance 를 생성할 수 있도록 mutable "companion class"
    를 작성할 수 있다. companion class 는 완벽한 구현이 가능하다는 전제하에 package-private 로 사용하면 되고, 아닌 경우 public 으로 선언하여
    client 가 구현하도록 하자 (e.g. `String` 의 public companion class `StringBuilder`)
    
* 위 단점은 nonfinal field 에 expensive computation 의 결과를 cache 하는 것으로 어느 정도 커버할 수 있다
