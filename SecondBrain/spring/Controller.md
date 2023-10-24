# Controller
* 클라이어트의 요청을 처리하고 응답을 반환하는 역할을 담당
* 클라이어트로부터 받은 요청을 해석하고, 해당 요청에 대한 비즈니스 로직 처리를 서비스 계층에 위임한다.
* **요청에 따라 적절한 서비스 메소드를 호출**하여 비즈니스 로직을 수행하고, 처리결과를 클라이언트에게 반환한다.
* 주로 사용자 인터페이스와의 상호작용을 담당한다.
* **요청 URL에 대한 매핑**과 **요청 처리**를 담당하는 메서드를 포함한다.

## 구현 방법
Controller임을 나타내기 위해서 class위에 @Controller 또는 @RestController Annotation을 붙이면 된다. API 개발을 진행할 때는 @RestController 를 사용한다.

`@RestController` = `@Controller` + `@ResponseBody` 이기 때문이다.