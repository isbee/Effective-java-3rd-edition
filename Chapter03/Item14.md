# Item 14 : Consider implementing `Comparable`

### `Comparable` 을 구현하면 다음과 같은 장점이 있다
* 다양한 generic algorithm 을 해당 class 에 적용할 수 있다
    * `sort`, `max`, `shuffle`, etc... 
* 다양한 Collection 과 융합할 수 있다
    * e.g. `TreeMap<K, V>` 의 K 로 사용

### `Comparable` 의 general contract
* `equals` 와 유사하게 reflexivity, symmetry, transitivity 를 준수해야 한다
    * object `x`, `y` 가 있을 때
        * `x` 가 `y` 보다 작으면 음의 정수를 반환해야 한다
        * `x` 와 `y` 가 같으면 0 을 반환해야 한다
        * `x` 가 `y` 보다 크면 양의 정수를 반환해야 한다
        * 'x', 'y' 의 type 이 다르고, 이것으로 인해 비교를 할 수 없게 된다면 `ClassCastException` 예외를 던져야 한다
    * `((x.compareTo(y)==0) == (x.equals(y))` 을 만족한다
        * **만족하지 않아도 상관없지만**, 그럴 경우 natural ordering 이 `equals` 와 inconsistent 하다고 명시해야 한다

* general contract 를 지키는 `compareTo` 구현시, class 내 field 에 대해 재귀적으로 `compareTo` 를 호출하자. 만일 `Comparable` 을 구현하지 않는 
field 가 있다면 `Comparator` 를 사용하자.
    
### 잘못된 `Comparable` 구현은 재앙이 될 수 있으니 조심하자
* 위 general contract 에서 `equals` 와의 consistency 를 지키지 않은 경우, **sorted collection 과 호환이 되지 않는다**
    * `Map`, `Set` 등의 sorted collection 은 'equality test' 를 `equals` 대신 `compareTo` 로 수행하기 때문이다
    
* `> < + -` 를 이용해서 구현하는 건 error-prone 이다. **`compare`** 같은 method 를 사용하자.
    * boxed primitive type 과 `Comparator` 등이 `compare` 지원
    * 특히 `Comparator` 의 경우 `comparingInt` 등 다양한 method 존재
