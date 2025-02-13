Database에 각 Table에 공통적으로 넣는 Column을 추상클래스로 만들어 활용할 수 있는 객체

```java
@MappedSuperclass 
@EntityListeners(AuditingEntityListener.class) 
public abstract class BaseTimeEntity { 
	@CreatedDate 
	private LocalDateTime createdAt; 
	
	@LastModifiedDate 
	private LocalDateTime updatedAt; 
}
```

위의 BaseEntity를 Column에 추가하고 싶다면 기존 Entity code에 BaseEntity를 상속하면 된다.
```java
@Entity 
@Getter 
@NoArgsConstructor(access = AccessLevel.PROTECTED) 
public class Member extends BaseTimeEntity { 
	... 
}
```
