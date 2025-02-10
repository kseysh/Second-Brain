HTTP 요청 메시지를 개발자가 직접 파싱해서 사용해도 되지만, 매우 불편하다. 
서블릿은 개발자가 HTTP 요청 메시지를 편리하게 사용할 수 있도록 개발자 대신에 HTTP 요청 메시지를 파싱하고 결고를 HttpServletRequest 객체에 담아서 제공한다.

## HttpServletRequest가 제공하는 정보
- START LINE
	- HTTP 메서드
	- URL
	- 쿼리 스트링
	- 프로토콜 (HTTP/1.1)
- 헤더
	- 헤더 조회
- 바디
	- form 파라미터 형식 조회
	- message body 데이터 직접 조회

### 추가 기능
#### 임시 저장소 기능
- 저장 : request.setAttribute(name, value)
- 조회 : request.getAttribute(name, value)