# Item 9 : Prefer try-with-resources to try-finally

### Try-with-resources 가 더 간결하고 debug 에 좋다
```java
   static void copy(String src, String dst) throws IOException {
       try (InputStream  in  = new FileInputStream(src);
            OutputStream out = new FileOutputStream(dst)) {
           
           byte[] buf = new byte[BUFFER_SIZE];
           int n;
           while((n = in.read(buf)) >= 0)
               out.write(buf, 0, n);
       }
   }
```
* try-finally 는 여러개의 자원을 다루는 경우 반복적인 try-finally 가 필요하지만, try-with-resources 는 한번에 처리할 수 있다

* 여러 개의 예외가 발생했다고 했을 때 try-finally 는 **마지막으로** 발생한 예외에 의해 다른 예외들이 말살되는 반면, try-with-resources 는 
**첫번째로** 발생한 예외가 나머지를 억압하므로 가장 먼저 발생한 문제를 해결할 수 있다.

* try-with-resources 를 사용할 때는 `AutoCloseable` 를 구현하는 것을 잊지 말자



***


### 요약
항상 try-with-resources 를 선호하자.
