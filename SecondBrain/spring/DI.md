# 의존성 주입(DI - Dependency Injection)

> 웹을 만들기 위해 Spring을 사용하는 것이 아니라 
"Spring은 DI를 이용하여 JAVA 어플리케이션을 만들 수 있도록 도와주는 프레임 워크이다. 여기에서 추가적으로 웹 MVC 모듈을 제공하여 웹을 효과적으로 만들 수 있도록 도와주는 것 뿐이다."

## 필드 주입 (@Autowired)
### 특징
- 코드가 간결해진다.
- 단, 외부에서 변경이 불가능하여 테스트하기 어려운 단점이 있다.
- DI 프레임워크가 없으면 아무것도 할 수 없다.
- 애플리케이션의 실제 코드와 상관없는 특정 테스트를 하고 싶을 때 사용한다.
- 정상적으로 작동되게 하려면 결국 setter가 필요하다.
- 만약, 의존관계를 필수적으로 넣지 않으려면 @Autowired(required=false) 옵션 처리를 통해 필수가 아님을 명시할 수 있다.

### 예시
1. 주입받으려는 빈의 생성자를 호출하여 빈을 찾거나 빈 팩토리에 등록
2. 생성자 인자에 사용하는 빈을 찾거나 생성
3. 필드에 주입

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

#### 순환 의존성을 알 수 없다.
`Constructor Injection`에서 순환 의존성을 가질 경우 BeanCurrentlyCreationExeption을 발생시킴으로써 순환 의존성을 알 수 있다.
## setter 주입

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

## 생성자 주입

### 특징
- 생성자 호출 시점에 1번만 호출되는 것을 보장한다.
- 불변과 필수 의존 관계에 사용한다.
- 생성자가 1개만 존재하는 경우 @Autowired를 생략해도 자동 주입된다.
- NPE(NullPointerException)를 방지할 수 있다.
- 주입받을 필드를 final로 선언 가능하다.
### 예시
1. 생성자의 인자에 사용되는 빈을 찾거나 빈 팩토리에서 생성
2. 찾은 인자 빈으로 주입하려는 생성자를 호출

```java
@Component
public class SampleService {
    private SampleDAO sampleDAO;
 
    @Autowired
    public SampleService(SampleDAO sampleDAO) {
        this.sampleDAO = sampleDAO;
    }
}

@Component
@RequiredArgsConstructor
public class SampleController {

	private final SampleService sampleService = new SampleService(new SampleDAO());
    
	...
}
```
	controller에서는 생성자 의존성 주입을 위해서 private final 선언과 @RequiredArgsConstructor를 사용해야한다.

### 장점
필수적으로 사용해야하는 의존성 없이는 Instance를 만들지 못하도록 강제할 수 있다

#### null을 주입하지 않는 한 NullPointerException 은 발생하지 않는다.
의존관계 주입을 하지 않은 경우에는 Controller 객체를 생성할 수 없다.
즉, 의존관계에 대한 내용을 외부로 노출시킴으로써 컴파일 타임에 오류를 잡아낼 수 있다.
#### final 을 사용할 수 있다.
final로 선언된 레퍼런스타입 변수는 반드시 선언과 함께 초기화가 되어야 하므로 setter 주입시에는 의존관계 주입을 받을 필드에 final 을 선언할 수 없다.
#### 순환 의존성을 알 수 있다.
`Field Injection`에서는 컴파일 단계에서 순환 의존성을 검출할 방법이 없지만, `Construtor Injection`에서는 컴파일 단계에서 순환 의존성을 잡아 낼 수 있다
#### 의존성을 주입하기가 번거로워 위기감을 느낄 수 있다.
`Construtor Injection`의 경우 생성자의 인자가 많아지면 코드가 길어지며 개발자로 하여금 위기감을 느끼게 해준다.

이를 바탕으로 SRP 원칙을 생각하게 되고, Refactoring을 하게 된다.

## 결론 : 생성자 주입을 사용하자

# 클래스 의존 관계에서의 DI의 장점
## 정적인 클래스 의존관계
정적인 의존관계는 클래스가 사용하는 import 코드만 보고도 의존관계를 쉽게 판단할 수 있다. 
ex)
![[Pasted image 20240211181127.png]]
## 동적인 클래스 의존관계
정적인 클래스 의존관계만으로는 실제 어떤 객체가 주입될 지 알 수 없다.
따라서 애플리케이션 실행 기점에 실제 새성된 객체 인스턴스의 참조가 연결된 의존 관계다.
![[Pasted image 20240211181416.png]]
애플리케이션 실해 시점에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결되는 것을 의존관계 주입이라 한다.
의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다.
> 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.

