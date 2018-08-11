# Item 12 : Always override `toString`

### `toString` 을 override 하여 의미있는 정보들을 반환하자 
* log 에 `Jenny=PhoneNumber@163b91` 대신 `Jenny=707-867-5309` 처럼 의미있는 정보들이 출력된다면 debugging 에 많은 도움이 된다

<br/>

### `toString` 이 반환하는 정보들은 모두 programmatic access 를 제공하자
* 해당 정보들을 getter 를 이용해 field 에 접근하지 못한다면 일일히 parse 해야 되는데, 이는 번거로울 뿐더러 error-prone 이다.

<br/>

### `toString` 을 override 하지 말아야 되는 경우
* Utility class 는 용도 상 `toString` 이 필요가 없으며, enum type 은 이미 훌륭한 `toString` 을 가지고 있다.

<br/>

### `toString` 역시 framework 로 생성할 수 있다
* `AutoValue` 나 IDEs 들이 생성해주는 걸 사용해도 무방하다

***

### 요약
superclass 의 `toString` 이 적합하지 않은 경우 instantiable class 는 항상 `toString` 을 override 하자. 
