# Item 4 : Enforce noninstantiability with a private constructor

### Utility class 는 그 목적에 따라 instance 생성이 불가능해야 바람직하다
* 따라서 이런 class 는 private constructor 만을 사용하도록 하자
* private constructor 덕분에 다른 class 가 utility class 를 상속할 수 없고, 이 역시 바람직하다
