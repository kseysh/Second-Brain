한 엔티티에서 같은 값 타입을 사용하면 컬럼 명이 중복되게 된다.
따라서 `@AttributeOverrides`,`@AttributeOverride`를 이용해서 컬럼 명의 속성을 재정의 하는 것이 좋다. 

ex)
```java
@Embedded
private Address homeAddress


private Address workAddress



```