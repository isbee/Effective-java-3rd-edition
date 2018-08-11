# Item 11 : Always override `hashCode` when you override equals

### equal objects 는 동일한 hash codes 를 가져야 한다
* 완벽하게 작성된 `eqauls` 가 2개의 객체가 동일하다는 것을 보장하더라도, hash code 가 다르면 다른 객체이다.

### 좋은 hash function 을 작성하는 방법
1. int 변수 `result` 를 선언하고 **첫번째 significant field**(`equals` 의 대상이 되는) 를 2.i 로 연산한 `c` 로 초기화 한다

2. 남아있는 **모든** significant fields 에 대해 아래 작업을 수행한다

    1. 해당 field 로 int 타입 hash code `c` 를 연산한다
        1. field 가 primitive type 이면 `Type.hashCode(f)`
        
        2. field 가 object reference 이면서, 이것의 `equals` 가 자신이 갖고 있는 field 에 재귀적인 `equals` 호출을 한다면 **똑같이**
        해당 field 의 `hashCode` 를 재귀적으로 호출하자. 만일 `equals` 가 좀 더 복잡한 구현으로 되있다면, 해당 field 의 
        **canonical representation** 에 대해 `hashCode` 를 호출한다. (e.g. null 의 canonical representation 은 0 )
        
        3. field 가 array 면 significant element 에 대해 위 2가지 rule 을 적용하고, 2.ii 로 hash code 값들을 결합한다. 
        만일 significant element 가 하나도 없다면 hash code 를 0 이 아닌 임의의 상수로 설정하고, 모든 요소가 significant 하다면 
        `Arrays.hashCode` 를 사용하자
        
    2. `c` 값을 `result` 와 결합한다
        * `result = 31 * result + c;`
        
3. `result` 를 반환한다 

<br/>

* **위 방법은 state-of-the-art 가 아니다.** `AutoValue`, `Object.hash`, `com.google.common.hash.Hashing` 등이 간편하면서도 
더 좋은 대안 일 수 있다.

### 성능 향상만을 목적으로 significant fields 를 hash code 연산에서 제외시키지 말자
* performance 향상이 있을 순 있지만 동일 hash code 가 나올 확률이 높아져 오히려 performance 가 낮아질 수 있다

***

### 요약
`eqauls` 를 override 한다면 반드시 `hashCode` 도 override 해야 한다.
