엔티티 클래스의 필드에 사용되며, 필드를 데이터베이스 테이블의 열에 매핑할 수 있다.
name 속성을 사용하여 매핑할 열의 이름을 지정할 수 있다.
다양한 다른 속성을 사용하여 데이터베이스 열에 대한 제약조건 및 속성을 정의할 수 있다.

### 예시
```
@Entity
@Table(name = "books")
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "title")
    private String title;
    
    // 다른 필드와 메서드 정의
}

```