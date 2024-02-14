# @Embeddable
`@Embeddable`은 JPA에서 엔티티 클래스의 일부 속성을 나타내기 위한 어노테이션입니다. 이를 사용하면 엔티티 클래스 내에 복합적인 속성을 정의할 수 있으며, 이러한 속성을 재사용하거나 구조화된 방식으로 저장할 수 있습니다.

간단히 말해, `@Embeddable`을 가진 클래스는 데이터베이스 테이블을 가지지 않는 독립적인 클래스입니다. 대신, 이 클래스의 속성은 다른 엔티티 클래스에서 사용될 수 있고, 데이터베이스 테이블의 컬럼으로 매핑됩니다.

예를 들어, 주소 정보를 나타내는 `Address` 클래스를 만들고 이를 엔티티 클래스인 `Person`에 포함하려면 다음과 같이 사용할 수 있습니다:

```java
@Embeddable 
public class Address {     
private String street;     
private String city;     
private String zipCode;          
// getter, setter, 생성자 등 
}
```