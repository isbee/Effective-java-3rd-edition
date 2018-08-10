# Item 10 : Obey the general contract when overriding `equals`

### `equals` 가 꼭 필요하지 않다면 override 하지 말자
* `equals` 를 잘못 사용하면 무서운 결과를 초래 할 수 있다. 따라서 'equals' 가 필요하지 않은 경우 override 하지 말자.
    * 각각의 instance 가 고유한 경우
    * "logical equality" 를 제공할 필요가 없는 경우
    * 이미 superclass 가 `equals` 를 override 했고, superclass 행동이 target class 와 일맥상통 하는 경우
    * target class 의 접근 제한이 private 혹은 default(package-private) 이면서, 해당 class 의 `equals` 가 호출 되지 않는다고 확신되는 경우

### `eqauls` 를 override 해야 한다면 general contract 를 지키자
* *Reflexive* : 모든 non-null reference value `x` 에 대해서 `x.equals(x) == true` 를 만족한다.

* *Symmetric* : 모든 non-null reference value `x, y` 에 대해서 `x.equals(y) == y.equal(x)` 를 만족한다.

* *Transitive* : 모든 non-null reference value `x, y, z` 에 대해서 `x.equals(y) == y.equal(z) == true` 라면 `x.equals(z) == true` 이다.

* *Consistent* : 모든 non-null reference value `x, y` 에 대해서 multiple invocation 이 일어나도 `x.equals(y)` 의 결과는 항상 동일해야 한다.

* 모든 non-null reference value `x` 에 대해서 `x.equals(null)` 은 반드시 false 를 반환해야 한다.

<br/>

* (*symmetric* 위반 예시) 책에는 general contract 를 위반한 다양한 예시들이 담겨 있지만 내용이 너무 많아서 생략한다.
   ```java
      public final class CaseInsensitiveString {
          private final String s;
          
          public CaseInsensitiveString(String s) {
              this.s = Objects.requireNonNull(s);
          }
          
          // Broken - violates symmetry!
          @Override public boolean equals(Object o) {
              if (o instanceof CaseInsensitiveString)
                  return s.equalsIgnoreCase( ((CaseInsensitiveString) o).s );
                  
              if (o instanceof String) // One-way interoperability!
                  return s.equalsIgnoreCase((String) o);
                  
              return false;
          }
          ...
      }
      ...
      
      CaseInsensitiveString cis = new CaseInsensitiveString("Polish");
      String s = "polish";
      
      cis.equals(s) // true
      s.equals(cis) // false, symmetry 위반
      
      
      // 이 문제를 해결하려면 equals 를 다음과 같이 변경
      @Override public boolean equals(Object o) {
          return o instanceof CaseInsensitiveString &&
                   ((CaseInsensitiveString) o).s.equalsIgnoreCase(s);
      }
   ```

### `equals` 를 작성하는 올바른 방법
1. == 연산자를 사용해서 argument 로 넘어온 reference 가 자기 자신인지를 확인하자
    * 자기 자신이라면 true 를 반환하면 된다
    
2. `instanceof` 연산자를 사용해서 `equals` 의 argument 가 올바른 type 인지를 확인하자
    * 올바른 type 이란 method 가 호출되는 class/interface 를 의미한다
    * 올바른 type 이 아니면 false 를 반환하자.
    
3. argument 를 올바른 type 으로 cast 하자
    * `instanceof` 검사가 선행되기 때문에 잘못된 cast 는 일어나지 않는다 (위의 symmetric 위반 예시 참고)
    
4. class 내의 모든 '중요한' field 가 `equals` 의 argument 와 일치하는지 확인하자
    * 만일 그렇다면 true 를 반환하고, 아니라면 false 를 반환하자
    * float, double 이외의 primitive type 은 == 로 검사하면 되고, object reference 는 재귀적으로 `equals` 를 호출하면 된다.
    * float, double 은 각각 `Float.compare` `Double.compare` 로 비교해야 하는데, 그 이유는 `Float.NaN`, -0.0f 같은 특수한 값들이 
    존재하기 때문이다
    
5. 항상 `hashCode` 를 override 하자 ([Item 11]())

6. `eqauls` 의 parameter 는 반드시 `Object` 여야 한다
    * `Object.equals` 의 parameter 가 `Object` 이기 때문이다. 따라서 `Object` 이외의 type 을 선언하면 **overriding 이 아닌 
    overloading 을 하게 된다.** ([Item 52]())

7. **Google 의 `AutoValue` framework 나 IDE 가 자동으로 생성해주는 `equals`, `hashCode` 를 사용 하는 것이 편하다** 
    
### `equals` 의 성능을 높히는 법
* 서로 다르기 쉬운 field, 또는 좀 더 쉽게 비교할 수 있는 field 에 대해서 우선적으로 비교하자
* 때로는 중요한 field 로 부터 계산되는 *derived fields* 를 비교하는 것이 더 빠를 수 있다

***

### 요약
`equals` 를 반드시 override 해야 되는 것이 아니라면 하지 말자. 있는 그대로 사용하는 것이 적합한 경우가 많다. override 해야 된다면 위에 언급된 사항들을 지키자
