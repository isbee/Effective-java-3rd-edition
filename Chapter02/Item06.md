# Item 6 : Avoid creating unnecessary objects

### 의도하지 않은 객체가 생성되는 경우가 종종 있다
```java
   public class RomanNumerals {
       private static final Pattern ROMAN = Pattern.compile(
               "^(?=.)M*(C[MD]|D?C{0,3})"
               + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
                      
       static boolean isRomanNumeral(String s) {
            return ROMAN.matcher(s).matches();
       }
   }
```
* `String.matches` 는 손쉽게 문자열 검사를 할 수 있지만 **내부적으로** `Pattern` **instance 를 생성하므로 performance 문제가 있다**
* 같은 regular expression 을 사용한다면 immutable 한 `Pattern` instance 를 미리 생성해 놓고 재사용하는 것이 바람직하다

<br/>

```java
   private static long sum() {
       Long sum = 0L;
       for (long i = 0; i <= Integer.MAX_VALUE; i++)
          sum += i;
       return sum;
   }
```
* Auto-boxing 은 불필요한 객체 생성이 일어날 확률이 매우 높다
* 위 예시는 sum 변수를 `Long` 으로 선언한 탓에 iteration 횟수만큼 불필요한 `Long` instance 가 생성된다
* 따라서 필요한 경우가 아니라면 boxed primitive type 대신 primitive type 을 쓰자
