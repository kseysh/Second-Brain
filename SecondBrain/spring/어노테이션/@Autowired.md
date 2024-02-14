## @Autowired란?

- `@Autowired` 어노테이션은 Spring의 의존성 주입(Dependency Injection)을 수행하기 위해 사용됩니다.
- 클래스 내의 필드, 생성자, 메서드에 `@Autowired`를 적용하여 해당 클래스에 필요한 빈(Bean)을 자동으로 주입받을 수 있습니다.
- 주로 서비스(Service) 클래스, 컨트롤러(Controller) 클래스, 그리고 다른 컴포넌트들 사이에서 의존성을 주입할 때 사용됩니다.

ex) 
```
@Service
public class MyService {
    private final MyRepository repository;

    @Autowired
    public MyService(MyRepository repository) {
        this.repository = repository;
    }// repository를 DI를 통해 주입해준 것

    // ...
}

```