# Effective-java-3rd-edition
* 본 repository 는 Effective java 3판을 1회독 한 후, 까먹지 않도록 내용 정리한 것입니다
* 시간이 날때 마다 정리 중이고 ~ 2018.08 내에 정리가 끝날 것 같습니다
* 본인이 이해하기 좋게 정리했기 때문에 **생략되거나 추가된 내용이 존재할 수 있습니다**
* 중요한 내용이나 용어들은 추후 관련 issue 검색을 고려해서 영어 그대로 가져왔습니다
* Chapter 1 은 introduction 이므로 여기서 짧게 정리

## Chapter 1 : Introduction
* 이 책은 performance를 다루기 보단 clear, maintainable, flexible 한 코드 작성법에 대해 다룬다
* 몇몇 chapter, item 이 성능에 관련되어 있긴 하지만 그것 자체를 clear 하게 사용하는 방법을 다룸

## Chapter 2 : Creating And Destroying Objects
* [Item 1](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter02/Item01.md) : 
Consider static factory methods instead of constructors
* [Item 2](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter02/Item02.md) : 
Consider a builder when faced with many constructor parameters
* [Item 3](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter02/Item03.md) : 
Enforce the singleton property with a private constructor or an enum type
* [Item 4](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter02/Item04.md) : 
Enforce noninstantiability with a private constructor
* [Item 5](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter02/Item05.md) : 
Prefer dependency injection to hardwiring resources
* [Item 6](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter02/Item06.md) : 
Avoid creating unnecessary objects
* [Item 7](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter02/Item07.md) : 
Eliminate obsolete object references
* [Item 8](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter02/Item08.md) : 
Avoid finalizers and cleaners
* [Item 9](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter02/Item09.md) : 
Prefer try-with-resources to try-finally

## Chapter 3 : Methods Common to All Objects
* [Item 10](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter03/Item10.md) : 
Obey the general contract when overriding `equals`
* [Item 11](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter03/Item11.md) : 
Always override `hashCode` when you override `equals`
* [Item 12](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter03/Item12.md) : 
Always override `toString`
* [Item 13](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter03/Item13.md) : 
Override `clone` judiciously
* [Item 14](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter03/Item14.md) : 
Consider implementing `Comparable`

## Chapter 4 : Classes and Interfacs
* [Item 15](https://github.com/isbee/Effective-java-3rd-edition/blob/master/Chapter04/Item15.md) : 
Minimize the accessibility of classes and members
