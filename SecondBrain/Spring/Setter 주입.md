## Setter 주입

### 특징
- 선택과 변경 가능성이 있는 의존 관계에 사용한다.
- 자바 빈 프로퍼티 규약의 수정자 메서드 방식을 사용하는 방법이다.
- set필드명 메서드를 생성하여 의존 관계를 주입한다.
- @Autowired를 입력하지 않으면 실행이 되지 않는다.

### 예시

1. 주입받으려는 빈의 생성자를 호출하여 빈을 찾거나 빈 팩토리에 등록
2. 생성자 인자에 사용하는 빈을 찾거나 생성
3. 주입하려는 빈 객체의 수정자를 호출하여 주입

```java
@Component
public class SampleController {
    private SampleService sampleService;
 
    @Autowired
    public void setSampleService(SampleService sampleService) {
        this.sampleService = sampleService;
    }
}
```

### 장점
`Setter Injection`은 선택적인 의존성을 사용할 때 유용하다.  
상황에 따라 의존성 주입이 가능하다.

### 단점
주입이 필요한 객체가 주입이 되지 않아도 얼마든지 객체를 생성할 수 있다 => 생성자 주입으로 해결 가능