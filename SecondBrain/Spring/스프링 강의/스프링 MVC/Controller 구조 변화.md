## V1 구조
[[Front Controller Pattern]]을 도입하여 프론트 컨트롤러가 요청에 맞는 컨트롤러를 호출하고, 공통 처리 기능을 담당한다.
프론트 컨트롤러만 서블릿을 사용한다.
![[Pasted image 20250218204804.png]]
## V2 구조
컨트롤러에서 뷰를 반환하는 부분에 중복이 있다. 공통 처리가 가능한 Front Controller에 역할을 위임한다.
Controller는 Front Controller만 의존하고, Front Controller가 view를 이용해 응답을 하도록 했다.
![[Pasted image 20250218204853.png]]

## V3 구조
컨트롤러가 서블릿 기술을 몰라도 되도록 HttpServletRequest, HttpServletResponse말고 요청
![[Pasted image 20250218205027.png]]

## V4 구조
![[Pasted image 20250218205055.png]]

## V5 구조
![[Pasted image 20250218205121.png]]