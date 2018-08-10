# Item 11 : Always override `hashCode` when you override equals

### equal objects 는 동일한 hash codes 를 가져야 한다
* 완벽하게 작성된 `eqauls` 가 2개의 객체가 동일하다는 것을 보장하더라도, hash code 가 다르면 다른 객체이다.

* hash code 를 생성하기 위한 hash function 을 작성하는 방법은 책에 나와 있긴 하지만 manual 하게 작성할 이유가 다소 떨어져서 **생략한다**
    * `AutoValue` 등이 제공하는 것을 쓰면 된다
    * 혹은 `Object.hash` 를 사용하자 (직접 작성한 것보다 performance 떨어짐)

### 중요한 fields 를 hash code 연산에서 제외시키지 말자
* performance 향상이 있을 순 있지만 동일 hash code 가 나올 확률이 높아져 오히려 performance 가 낮아질 수 있다

***

### 요약
`eqauls` 를 override 한다면 반드시 `hashCode` 도 override 해야 한다.
