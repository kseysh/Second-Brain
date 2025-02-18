## V1 구조
- [[Front Controller Pattern]]을 도입하여 프론트 컨트롤러가 요청에 맞는 컨트롤러를 호출하고, 공통 처리 기능을 담당한다.
- 프론트 컨트롤러만 서블릿을 사용한다.
![[Pasted image 20250218204804.png]]
## V2 구조
- 뷰를 처리하는 객체 생성
	- 컨트롤러에서 뷰를 반환하는 부분에 중복이 있다. 공통 처리가 가능한 Front Controller에 역할을 위임한다.
	- Controller는 Front Controller만 의존하고, Front Controller가 view를 이용해 응답을 하도록 했다.
![[Pasted image 20250218204853.png]]

## V3 구조
- 컨트롤러가 서블릿 기술을 몰라도 되도록 하였다. (컨트롤러의 서블릿 종속성 제거)
	- HttpServletRequest 말고 요청 파라미터를 Map으로 넘기도록 하였다.
	- HttpServletResponse말고 응답 객체를 만들어 넘기도록 하였다.
- 뷰리졸버를 이용해 컨트롤러는 뷰의 논리 이름을 반환하고, 실제 물리 위치의 이름은 프론트 컨트롤러에서 처리하도록 한다.
![[Pasted image 20250218205027.png]]

## V4 구조
- 컨트롤러가 ModelView가 아닌 ViewName만을 반환하도록 한다.
- model 객체를 프론트 컨트롤러에서 생성해서 넘겨주고, 컨트롤러에서는 모델 객체에 값을 담기만 한다.
![[Pasted image 20250218205055.png]]

## V5 구조
![[Pasted image 20250218205121.png]]