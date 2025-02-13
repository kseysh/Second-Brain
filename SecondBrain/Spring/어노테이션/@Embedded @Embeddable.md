1. **@Embeddable:**
    
    - `@Embeddable` 어노테이션은 해당 클래스가 다른 엔터티 클래스에 임베디드(포함)될 수 있다는 것을 나타냅니다.
    - 일반적으로 값 객체(Value Object)를 나타내는 클래스에 사용됩니다. 값 객체는 데이터의 묶음이며, 자체적으로 식별자를 가지지 않습니다.
    - 예를 들어, 주소나 이름과 같이 하나의 엔터티에 속하는 부분적인 정보를 표현할 때 사용됩니다.
    

    
```
@Embeddable 
public class Address {     
	private String street;     
	private String city;     
	private String zipCode;     
	
	// 생성자, 게터, 세터 등 필요한 메서드를 작성 
}
```
    
2. **@Embedded:**
    
    - `@Embedded` 어노테이션은 실제 엔터티 클래스 안에서 `@Embeddable` 어노테이션이 붙은 클래스를 포함한다는 것을 나타냅니다.
    - 엔터티 클래스의 필드로 `@Embedded` 어노테이션을 사용하면, 해당 필드에 `@Embeddable`로 표시된 클래스의 속성들이 포함됩니다.
    - `@Embedded`는 컬럼이 아니라 값 객체를 포함하는 데 사용됩니다.
    

    
```
@Entity 
public class User {     
	@Id     
	@GeneratedValue(strategy = GenerationType.IDENTITY)     
	private Long id;          
	private String username;      
	@Embedded     
	private Address address;      
	// 생성자, 게터, 세터 등 필요한 메서드를 작성 
}
```

위의 예제에서 `User` 엔터티는 `Address`를 포함하고 있습니다. 이때 `@Embedded` 어노테이션이 사용되었으며, `Address` 클래스는 `@Embeddable` 어노테이션이 부여되어 값 객체로 사용됩니다.