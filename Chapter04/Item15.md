# Item 15 : Minimize the accessibility of classes and members

### 접근 제한자를 minimize 해야 하는 이유
* API 제공시 최대한 decoupling 해서 내부에 수정이 발생해도 client 가 신경 쓸 필요가 없도록 해야 한다
* decoupling 을 하려면 encapsulation 이 필요하다

### 접근 제한자를 minimize 하는 법
* top-level(non-nested) class 는 `public` 또는 `package-private(=default)` 밖에 선택사항이 없다
    * API 의 공개 대상 class 라면 `public`, 구현용 class 라면(반복적인 수정이 필요하고, 또는 없어질 수도 있는 class) `package-private`
    
    * 구현용 class 가 오직 한 군데 에서만 쓰인다면, 다른 source file 의 top-level class 로 선언하기 보다 같은 source file 의 
    private static nested 로 선언하자 ([Item 24]())

* 모든 class 의 field 는 가능한 `private` 로 선언한다
    * 특히 mutable field 는 기본적으로 thread-safe 하지 않으므로 반드시 `private` 로 선언
    * immutalbe class 는 final 키워드도 필수적이다 ([Item 17]())

* *(유의 사항)* public static final 을 조심하자. 해당 field 가 container 인 경우 container 의 reference 는 수정 불가능 하지만 
**element 는 수정 가능하므로** 외부에서 문제를 일으킬 수 있다

* *(유의 사항)* **`Serializable` 을 구현하는 class 는 `private` field 도 노출될 수 있다** ([Item 86](), [Item 87]())

### 필요에 따라 *module system* 을 같이 사용하자
* *module system* 은 java 9 에 release 됬다. 여기서 moudle 은 package 의 모음을 뜻하며 export declaration 을 통해 
package 의 일부분을 export 할 수 있다.

* 어떤 module 의 unexported packages's `public`, `protected` 멤버는 moudle 외부에서 접근할 수 없지만, module **내부에서는**
이런 export declaration 에 영향을 받지 않는다.
    * 따라서 module 을 사용하면 **외부의 접근을 제한하면서 동시에 package 간의 class 공유가 가능하다**
    
* 다만 많은 경우 package 를 잘 정리하면 module system 이 필요 없다. 
    * 따라서 비교적 최근에 공개된 module system 을 바로 채용할 필요는 없다. 
    
