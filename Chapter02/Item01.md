# Item 1 : Consider static factory methods instead of constructors

### Static factory method 와 factory method pattern 가 다른 것이라는 걸 인지하자
* Factory method pattern 은 객체 생성을 담당하는 ‘factory’ 를 생성한 후, 그 factory 가 객체를 생성하는 것이다.
* 반면에 static factory method는 ‘factory’ 없이 바로 객체를 생성.

### Static factory method 는 factory pattern 의 장점들을 그대로 가져간다
* method 이름이 있으므로 가독성이 좋아진다
* invocation 이 일어나도 경우에 따라 객체 생성을 하지 않아도 무관하다
    * `Boolean.valueOf()` 는 boolean 을 반환 
    * 자주 쓰이는 **immutable 객체를 cache** 하면 반복적으로 객체 생성할 필요가 없다
* 여러 subtype 객체를 return 할 수 있다
    * `HashSet<E>` 대신 `LinkedHashSet<E>`
* method parameter 에 따라 return 하는 object 를 다르게 할 수 있다
* static factory method 작성 시점에 return 대상인 object 가 존재하지 않아도 된다
    * JDBC 의 `getConnection()`

### Static factory method 는 다음과 같은 단점이 있다
* 만일 public 또는 protected constructor 를 제공하지 않고 static factory method 만을 사용한다면, 해당 class 는 subclass 가 불가능하다
    * composition 으로 해결 ([Item 18]())
* API 문서에서 static factory method 를 찾기 힘들다


***

### 요약
Static factory method 와 public constructor 모두 각자의 쓰임새가 있다. 다만 static factory 가 더 선호할 만 하다.

