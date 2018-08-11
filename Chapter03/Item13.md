# Item 13 : Override `clone` judiciously

### `Cloneable` 에는 결함이 있다
* `Cloneable` 은 `clone` method 가 없는 mixin interface 다. 따라서 어떤 객체가 `Cloneable` 을 구현했더라도 reflection 을 통해 
`clone` 을 호출할 수 있다는 **보장이 없다.**

* 일반적으로 interface 를 구현한다는 것은 class 가 '어떤 일을 할 수 있는가' 를 말해주는 것인데, `Cloneable` 은 superclass `Object` 의 
protected method `clone` 을 수정하기 위한 것이므로 굉장히 atypical 하다.

### Copy constructor, copy factory 라는 좋은 대안이 있다
* 아래에서 다루는 여러가지 사항들을 고려하지 않아도 된다
* interface 를 argument 로 받아서 `HashSet<E>` 을 `TreeSet<E>` 으로 복사하는 등, 보다 자유로운 구현이 가능하다. 
(*conversion constructor, conversion factory*)

### 그럼에도 `Cloneable` 을 구현하고 `clone` 을 override 하고 싶다면,
* `clone` 의 general contract 를 지키자
    * 모든 object `x` 는 `x.clone() != x` 를 만족한다
    * 모든 object `x` 는 `x.clone().getClass() == x.getClass()` 를 만족한다 (**필수 조건은 아님**)
    * 모든 object `x` 는 `x.clone().equals(x) == true` 를 만족한다 (**필수 조건은 아님**)
    
* (참고) immutable class 는 재사용의 대상이므로 `clone` 을 **제공하면 안된다**

### `clone` 이 original object 의 상태 변화를 일으켜선 안된다
```java
@Override public Stack clone() {
    try {
        Stack result = (Stack) super.clone();
        
        // Stack 내부의 elements 배열도 재귀적으로 clone 해야
        // 같은 reference 를 공유하지 않는다
        result.elements = elements.clone();
        return result;
    } catch (CloneNotSupportedException e) {
        throw new AssertionError();
    }
}
```
* 위 예시에서 `elements` 를 `clone` 할 수 있었던 것은 final field 가 아니였기 때문이다. `Cloneable` architecture 의 문제는 
**final field 는 `clone` 할 수 없고,** 하고 싶은 경우 final 을 제거해야 한다는 것이다 

* **재귀적인 `clone` 을 해도 reference 가 공유 될 수 있다.** 예를 들어 `elements` 가 primitive type 배열이 아니라 특정 객체 배열이었다면,
각 요소의 객체 reference 가 공유 되버린다.
    * 이 경우 각각의 객체를 다시 deep copy 해야 한다

* constructor 와 동일하게 `clone` 은 내부적으로 **overridable method 를 호출하면 안된다.** 왜냐하면 `clone` 이 종료되기 전에 
subclass method 가 실행되서 복사를 방해하거나, original object 를 훼손시킬 수 있기 때문이다.

### 그 외 주의사항
* `Object.clone` 은 `CloneNotSupportedException` 예외를 던지지만 **override 한 public `clone` 은 예외를 던지지 말아야 한다.** ([Item 71]())
* synchronization 을 지원하지 않기 때문에 multi-threading 환경에서는 `synchronized` 가 필요하다.


