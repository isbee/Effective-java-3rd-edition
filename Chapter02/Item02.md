# Item 2 : Consider a builder when faced with many constructor parameters

### Builder 패턴을 사용하지 않으면, 많은 parameter 를 필요로 하는 object 를 생성할 때 불편하다
* 서로 다른 parameter 의 constructor 를 overloading 하는 **telescoping** 패턴
    * 가독성이 떨어지고 client 쪽에서 code 작성이 불편해짐
* setter 를 이용한 **JavaBean** 패턴
    * instance 생성 시점에 모든 validity 를 점검할 수 없으므로 **inconsistent state** 에 놓일 수 있다
    * **immutable class 를 만들 수 없다**

### Builder 패턴을 사용하면 위와 같은 문제를 해결할 수 있다
* python 의 named parameter, optional parameter 와 비슷한 형태로 사용하므로, 가독성이 좋고 client 가 사용하기 편하다
    * `new Pizza.Builder(SMALL).addTopping(SAUSAGE).build()`

* abstract class 는 abstract builder 를, concrete class 는 concrete builder 를 갖게 할 수 있으므로 **class hierarchy** 구현에 좋다
    * ```java
         public abstract class Pizza {
             public enum Topping { HAM, MUSHROOM, ONION, PEPPER, SAUSAGE }
             final Set<Topping> toppings;
  
             // abstract class 와 abstract builder
             abstract static class Builder<T extends Builder<T>> {
                 EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);

                 public T addTopping(Topping topping){
                     toppings.add(Objects.requireNonNull(topping));
                     return self();
                 }
        
                 abstract Pizza build();

                 // subclass must override this method to return "this"
                 protected abstract T self();
             }
             Pizza(Builder<?> builder){
                 toppings = builder.toppings.clone(); // See Item 50 (defensive copy)
                                                      // Item 13 에 따라 clone 보다 EnumSet.copyOf 가 바람직
             }
         }

         public class NyPizza extends Pizza {
             public enum Size { SMALL, MEDIUM, LARGE }
             private final Size size;

             // concrete class 와 concrete builder
             public static class Builder extends Pizza.Builder<Builder> {
                 private final Size size;

                 public Builder(Size size){
                     this.size = Objects.requireNonNull(size);
                 }
                 @Override public NyPizza build() {
                     return new NyPizza(this);
                 }
                 @Override protected Builder self() { return this; }
             }
             private NyPizza(Builder builder){
                 super(builder);
                 size = builder.size;
             }
         }
      ```

### Builder 패턴은 다음과 같은 단점을 가지고 있다
* Object 를 만들기 위해 **먼저 builder 를 생성해야 하므로** performance-critical 한 상황에서는 문제가 될 수 있다
    * 대부분의 상황에서는 지장 없다
* telescoping 패턴 보다 더 verbose 하다
    * parameter 갯수가 4개 이상인 경우에만(미래의 수요까지 고려해서) 사용하자


***


### 요약
많은 parameter(특히 optional 하거나 identical 한) 를 필요로 하는 constructor 나 static factory 를 작성하는 것 보다는 builder 를 사용하는것이 좋다. 
