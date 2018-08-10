# Item 5 : Prefer dependency injection to hardwiring resources

### Flexibility 와 testability 를 제공하기 위해서는 class 내부에서 모든 것을 정의하기 보다 외부에서 dependency 를 받아오자
```java
   public class SpellChecker {
       private final Lexicon dictionary;
       
       public SpellChecker(Lexicon dictionary) {
           this.dictionary = Objects.requireNonNull(dictionary);
       }
       ...
   }
```
* parameter 로 factory 를 받아온다면 더욱 좋다



***


### 요약
class 내부에서 직접적으로 자원을 생성하기 보다 dependency injection 방식을 사용하자. 재사용성, 유연성 등 장점이 많다
