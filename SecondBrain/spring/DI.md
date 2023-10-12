# 의존성 주입(DI - Dependency Injection)

> 웹을 만들기 위해 Spring을 사용하는 것이 아니라 
"Spring은 DI를 이용하여 JAVA 어플리케이션을 만들 수 있도록 도와주는 프레임 워크이다. 여기에서 추가적으로 웹 MVC 모듈을 제공하여 웹을 효과적으로 만들 수 있도록 도와주는 것 뿐이다."

## 생성자 주입

## setter 주입

## 필드 주입 (@Autowired)

### 예시
변수 선언부에 @Autowired Annotation을 붙인다.

```java
@Component
public class SampleController {
    @Autowired
    private SampleService sampleService;
}
```

### 단점
#### 단일 책임(SRP)의 원칙 위반
#### 의존성을 주입하기가 쉽다. 
	- @Autowired 선언 아래 개수 제한 없이 무한정 추가할 수 있기 때문이다.
#### 의존성이 숨는다
`DI Container`를 사용한다는 것은 Class가 자신의 의존성만 책임진다는게 아니라 제공된 의존성 또한 책임진다.
Class가 어떤 의존성을 책임지지 않을 때, 메서드나 생성자를 통해(Setter나 Constructor) 확실히 커뮤니케이션이 되어야한다.
하지만 `Field Injection`은 숨은 의존성만 제공해준다
#### DI Container의 결합성과 테스트 용이성
`DI Framework`의 핵심 아이디어는 관리되는 Class가 `DI Container`에 의존성이 없어야 한다.

즉, 필요한 의존성을 전달하면 독립적으로 Instance화 할 수 있는 단순 POJO여야한다.

`DI Container` 없이도 Unit Test에서 Instance화 시킬 수 있고, 각각 나누어서 테스트도 할 수 있다.

`Container`의 결합성이 없다면 관리하거나 관리하지 않는 Class를 사용할 수 있고, 심지어 다른 `DI Container`로 전환할 수 있다.

하지만, `Field Injection`을 사용하면 필요한 의존성을 가진 Class를 곧바로 Instance화 시킬 수 없다.

#### 불변성(Immutability)
`Constructor Injection`과 다르게 `Field Injection`은 final을 선언할 수 없다.
그래서 객체가 변할 수 있다.
#### - **순환 의존성**

`Constructor Injection`에서 순환 의존성을 가질 경우 BeanCurrentlyCreationExeption을 발생시킴으로써 순환 의존성을 알 수 있다.

