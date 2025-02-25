스프링 MVC에서 핸들러 밖으로 예외가 던져진 경우 예외를 해결하고 동작을 새로 정의하는 방법

## 동작 흐름
ExceptionResolver가 없다면, WAS에서 `/error`로 다시 요청을 보낸다.
ExceptionReslover를 적용하면 DispatcherServlet이 예외 해결을 위해 ExceptionResolver를 호출하고, ExceptionResolver가 ModelAndView를 다시 반환하면서 정상 흐름으로 돌아온다. (원래 핸들러가 정상 호출을 했다면 ModelAndView를 어차피 반환할 것이였으므로)
ExceptionResolver를 사용하면 예외가 발생해도 서블릿 컨테이너까지 예외가 전달되지 않는다. (ExceptionHandler이므로 이름 그대로 Exception을 해결하여 정상 흐름처럼 변경하는 것이 목적)
![[Pasted image 20250225195617.png|400]]
### 활용
- 예외 상태 코드 변환
- 뷰 템플릿 처리
- API 응답 처리

## 스프링이 제공하는 ExceptionResolver
- ExceptionHandlerExceptionResolver
	- `@ExceptionHandler`처리
- ResponseStatusExceptionResolver
	- HTTP 상태 코드 지정 (@ResponseStatus, ResponseStatusException 예외)
	- sendError(400)를 호출한다.
- DefaultHandlerExceptionResolver
	- 스프링 내부 기본 예외 처리
	- sendError(400)을 호출한다.