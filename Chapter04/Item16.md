# Item 16 : In public classes, use accessor methods, not public fields

### package 외부에서 접근 가능한 class 는 getter, setter 를 사용하자
* client 가 직접적으로 field 에 접근한다면 향후 class 수정이 어려워지지만, getter 로 간접적으로 접근한다면 field 를 마음대로 변경할 수 있다

### public field 를 사용해도 괜찮은 경우
* package-private class 와 private nested class 는 API 로 노출되지 않기 때문에 개발자가 적절하게 사용한다는 전제하에 public field 를
사용해도 좋다.
    * getter/setter 가 없어지므로 class 가 더 간단해지는 면도 있다

### 요약
public class 는 mutable fields 를 외부에 노출시키면 안된다. **immutable fields 는 노출시켜도 문제는 없지만 좋지 않은 패턴이다.** 
하지만 package-private class 나 private nested class 는 public field 를 사용하는 것이 좋을 수도 있다.
