엔티티 클래스에 사용되며, `@Table`을 통해 엔티티 클래스를 특정 데이터베이스 테이블과 매핑할 수 있다. `name`속성을 사용하여 매핑할 데이터베이스 테이블의 이름도 지정할 수 있다.

### 예시
```
@Entity
@Table(name = "books")
public class Book {
    // 엔티티 클래스의 필드 및 메서드 정의
}
```