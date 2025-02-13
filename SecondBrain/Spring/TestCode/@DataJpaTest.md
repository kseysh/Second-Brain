## @DataJpaTest를 사용했을 때 Repository Layer 단위 테스트의 특징.

- @DataJpaTest를 사용하게 되면 모든 테스트 메서드에 @Transactional이 적용되고 @Test 환경에서는 default로 commit 된다.(각각의 테스트 메서드는 다른 메서드에 영향을 주어서는 안되기 때문에 매 테스트 마다 데이터를 초기화 해준다.)
- @BeforeEach와 각각의 메서드가 하나의 트랜잭션으로 묶이게 된다.
- EmbeddedDB 인 H2 DB를 내장으로 AutoConfigure하여 등록해주기 때문에 이를 활용해도 좋고 DB종류에 종속적인 테스트 내용이 있다면 Custom 으로 DB를 연결할 수 있다.(이 경우 @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)로 설정한 후 application.yml에 db설정 정보 추가.)

## @DataJpaTest가 수행하는 3가지 역할

```null
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@BootstrapWith(DataJpaTestContextBootstrapper.class)
@ExtendWith(SpringExtension.class)
@OverrideAutoConfiguration(enabled = false)
@TypeExcludeFilters(DataJpaTypeExcludeFilter.class)
@Transactional
@AutoConfigureCache
@AutoConfigureDataJpa
@AutoConfigureTestDatabase
@AutoConfigureTestEntityManager
@ImportAutoConfiguration
public @interface DataJpaTest {
```

1. EntityManager, Jpa관련 Repository 객체등을 Auto Configure해준다. 즉, Jpa Repository와 관련된 객체를 빈으로 등록해준다.(@SpringBootApplication annotation을 찾고 ApplicationContext를 로딩하는 점은 @SpringBootTest와 동일하다.)
2. test database를 Auto Configure해준다. 디폴트로 H2 DB를 인메모리 형태로 실행하여 테스트할 수 있게 해준다. 만약 custom DB를 사용하고 싶을 경우 database 자동설정 어노테이션을 오버라이딩하고 application.yml 에 datasource정보를 등록하여 사용할 수 있다.
    
    ```null
    @DataJpaTest
    @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
    class ArticleRepositoryCustomImplTest {
    ```
    
3. transaction을 걸어주고 기본 옵션으로 rollback을 수행한다. rollback을 원하지 않을 경우 각 테스트케이스 메서드에 Rollback 어노테이션을 써야한다.

```null
@Test
@Rollback(false)
void test(){}
```