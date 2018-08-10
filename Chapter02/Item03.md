# Item 3 : Enforce the singleton property with a private constructor or an enum type

### Singleton class 를 작성할때는 다음과 같은 사항을 고려해야 한다
* Singleton class 가 serializable 하려면 `Serializable` 을 구현하는 것 뿐만 아니라 **모든 instance field 를** `transient` **로 선언하고** 
`readResolve` **method 를 제공해야 한다**([Item 89]())

* `reflection` 을 사용하면 privileged client 가 **private constructor 를 invoke 할 수도 있다**

### Enum singleton 은 위 사항들이 전혀 문제가 되지 않는다
```java
   public enum Elvis {
       INSTANCE;
       
       public void leaveTheBuilding() { ... }
   }
   ...
   
   Elvis.INSTANCE.leaveTheBuilding()
```
* Enum constants 는 public static final field 이다
* 복잡한 serialization 이 발생하거나, reflection 공격을 받더라도 enum instance 는 반드시 singleton 임을 보장한다
* 다만 Enum singleton 이 `Enum` 이외의 superclass 를 상속하는 것은 불가능하다.
    * Composition 으로 극복할 수 있다



***


### 요약
Singleton 을 구현하는 가장 좋은 방법은 single-element enum type 을 사용하는 것이다.
