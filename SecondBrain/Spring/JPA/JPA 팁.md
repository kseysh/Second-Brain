### 1. 모든 연관관계는 가능한 지연로딩으로
@ManyToOne, @OneToOne은 별도 설정이 없으면 기본이 즉시로딩이니 주의
### 2. IDE에서 디버그 모드로 실행 시, 연관관계가 자동 지연로딩 될 수 있다.
### 3. 지연로딩 사용 시 Service 레이어에 '@Transactional' 어노테이션 사용
### 4. 일대일 관계는 주의 필요 :: optional=false 이거나, 외래키 컬럼을 가지고 있는 경우만 지연로딩
null은 프록시로 감쌀 수 없다.
따라서 null인지 확실히 알 수 있어야 지연로딩을 해준다.
null인지 알 수 없으면 지연로딩으로 설정해도 즉시로딩될 수 있다.
### 5. 엔티티 클래스를 final로 선언하지 않는다.
final 클래스는 프록시 생성이 불가능하기 때문
### 6. Not null 컬럼 매핑 시 optional=false 조건 추가
연관관계 fetch join시 기본으로 outer join을 하는데, optional=false인 경우 inner join을 한다.
### 7. 엔티티 직렬화 시, 자동으로 지연로딩 발생할 수 있다.
ex) 레디스 같은 캐시 시스템에 엔티티를 캐싱하는 경우
직렬화하기 전 연관된 모든 엔티티를 모두 fetch join 하거나, 연관 엔티티를 직렬화에서 제외한다.
가능하다면 엔티티를 직렬화하기보다, DTO를 직렬화하는 것이 좋다.
### 8. toString()에 연관관계 엔티티 제외 필요
toString() 사용시 연관된 엔티티를 포함하면 불필요한 즉시 로딩이 발생할 수 있다.
@ToString에서 연관관계가 있는 필드는 exclude 옵션을 사용하여 제외시켜야 한다.
toString을 쓸 일이 없더라도, 라이브러리에서 내부적으로 사용하고 있을 가능성이 있어 처리하는 것이 좋다.
### 9. Auto-increment PK :: Bulk Insert는 JPA가 아닌 JDBC로
JPA는 `IDENTITY` 전략 엔티티인 경우 bulk insert 지원이 잘 안 된다.  
`saveAll()`이라는 메소드가 있지만, insert 쿼리가 엔티티마다 각각 나갑니다.
단일 쿼리로 여러개 insert 하고 싶을 경우, `JdbcTemplate`을 사용해야 한다.
### 10. ‘진짜’ 네이티브 쿼리를 로깅하도록 설정하기 (for MySQL)
쿼리 로그 확인시 hibernate 로그 설정을 하면 실제 동작하는 쿼리와 다르게 로깅하는 경우가 있다.
- 특히 bulk insert의 경우, 실제론 bulk insert 쿼리로 나가고 있음에도 insert 쿼리가 각각 나가는 것처럼 출력되는 이슈가 있다.
MySQL이 남겨주는 네이티브 쿼리를 직접 보는게 가장 정확하므로 아래 설정을 하는 것이 좋다.
```yaml
spring:
  datasource:
    hikari:
      jdbc-url: jdbc:mysql://localhost:3306/hibernate_batch?rewriteBatchedStatements=true&profileSQL=true&logger=Slf4JLogger&maxQuerySizeToLog=999999
```
- `rewriteBatchedStatements=true` : bulk insert 쿼리를 허용하는 설정
- `profileSQL=true`: 드라이버에서 전송하는 쿼리 출력 허용 설정
- `logger=SLF4J`: 기본값이 `System.err` 로 출력하도록 설정되어 있기 때문에 필수로 지정 필요 (MariaDB 드라이버일 경우 불필요)
- `maxQuerySizeToLog=999999` : 출력할 쿼리 길이 (기본값은 0이기에 필수 지정)
### 11. JPA로 bulk 쿼리 시 필수 어노테이션 :: @Modifying(clearAutomatically = true, flushAutomatically = true)
해당 어노테이션 사용 시, 쿼리 실행 시 flush와 함께 영속성 컨텍스트를 비워줍니다.  
`@Query`처럼 쿼리를 직접 지정한 벌크연산의 경우 JPA가 영속성 컨텍스트 업데이트를 안 해주기 때문에, DML 벌크 쿼리 시 반드시 필요한 어노테이션입니다.

미사용 시, 예를 들면 삭제 쿼리가 나갔음에도 엔티티 영속은 되어있기에 조회가 되는 버그가 생길 수 있습니다.
### 12. flush는 commit이 아니다

flush는 DB에 쿼리가 나가긴 하지만, 트랜잭션을 commit해주는건 아니기에  
DB 격리수준이 `READ COMMITTED` 이상일 경우, 다른 작업에서 쿼리 결과를 조회할 순 없습니다.
### 13. saveAndFlush() 보다는 변경 감지를 사용한다.
쿼리 횟수와 결과물만 본다면 둘이 같습니다.  
그러나 변경감지가 훨씬 Hibernate 최적화가 잘 되어있습니다.

또한 변경감지는 한번의 flush 작업으로 여러 엔티티를 반영할 수 있는데 반해,  
`saveAndFlush()`는 호출될 때마다 모든 엔티티의 변경 여부를 확인해야 합니다.
### 14. Auto-increment PK를 가지는 엔티티 생성 시, saveAndFlush() 는 불필요하다.
save만 해도 flush까지 됩니다. (auto_increment 키 값을 할당하기 위해)
### 15. Auto-increment PK 키 매핑 시 GenerationType.AUTO 쓰지 않는다.
`AUTO` 전략일 때 sequence를 지원하지 않는 DB를 사용하는 경우,  
Hibernate는 `TABLE` 전략을 사용합니다.
- TABLE은 별도의 Key 생성 전용 Table을 생성하고, 키 생성 때마다 해당 테이블에 비관적 락을 걸며 유니크한 키를 만들어주는 전략입니다. (DB 벤더에 중립적이라는 장점이 있습니다.)
확장성, 성능 면에서 손해볼 수 있으므로, `GenerationType.IDENTITY`로 정확히 지정해줍시다.
### 16. 조회만 할 시, DTO projection으로 필요한 데이터만 select하는 것이 성능이 좋다.



https://velog.io/@wisepine/JPA-사용-시-19가지-Tip