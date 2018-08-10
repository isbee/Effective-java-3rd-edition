# Item 7 : Eliminate obsolete object references

### Memory leak 을 null 로 방지할 수 있다
```java
   public class Stack {
       private Object[] elements;
       private int size = 0;
       private static final int DEFAULT_INITIAL_CAPACITY = 16;
       
       public Stack() {
           elements = new Object[DEFAULT_INITIAL_CAPACITY];
       }
       
       public void push(Object e) {
           ensureCapacity();
           elements[size++] = e;
       }
       public Object pop() {
           if (size == 0)
               throw new EmptyStackException();
           //return elements[--size];  // memory leak
           
           // null 을 통해 memory leak 방지
           Object result = elements[--size];
           elements[size] = null;
           return result;
       }
       private void ensureCapacity() {
           if (elements.length == size)
               elements = Arrays.copyOf(elements, 2 * size + 1);
       }
   }
```
* reference 가 자신의 scope 를 벗어나면 자연스럽게 garbage collecting 이 되지만, 위 예시는 `Stack` instance 가 유지되는 한 
`elements` 는 dereferend 되지 않는다
* 따라서 null 로 쓸데없는 reference 를 없애줘야 `push()` 를 통해 늘어난 reference 가 일으키는 memory leak 을 방지할 수 있다

### Cache 도 memory leak 을 유발하기 쉽다
* 객체 reference 를 cache 해놓더라도 일정시간 사용되지 않는다면 해제해야 하는데, 이를 까먹기 쉽다
* 이런 경우 `WeakHashMap`, `LinkedHashMap.removeEldestEntry()` 등을 고려하자

### Callback 도 memory leak 을 유발한다
* callback 을 weak reference 로 저장해서 garbage collecting 대상이 되도록 해야 하고, 역시 `WeakHashMap` 이 좋을 수 있다

