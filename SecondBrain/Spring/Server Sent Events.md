# 비교할 기술
## Short Polling
![[Pasted image 20251126152545.png|300]]
클라이언트가 주기적으로 서버로 데이터가 갱신되었는지 확인하는 것
## Long Polling
![[Pasted image 20251126152624.png|300]]
요청을 보내고 서버에서 변경이 일어날 때까지 대기하는 방법
실시간 메시지 전달이 중요하지만 서버의 상태가 빈번하게 변하지 않는 경우에 적합
서버로부터 응답을 받고 나면 다시 연결 요청을 한다.
## Web Socket
서버와 양방향 통신을 할 수 있는 방식
웹소켓 형식으로 HTTP를 업그레이드 한 후 사용해야 한다.
# Server-Sent Events
![[Pasted image 20251126152804.png|300]]
서버와 한 번 연결을 맺고나면 일정 시간동안 서버에서 변경이 발생할 때마다 데이터를 전송받는 방법
Long Polling 처럼 응답마다 다시 요청을 하지 않아도 된다.
## SSE 통신 개요
Client의 SSE Subscribe 요청
```http
GET /connect HTTP/1.1
Accept: text/event-stream
Cache-Control: no-cache
```
Server의 Subscription에 대한 응답
```http
HTTP/1.1 200
Content-Type: text/event-stream;charset=UTF-8
Transfer-Encoding: chunked
```
클라이언트에서 subscribe을 하고나면 서버는 해당 클라이언트에게 비동기적으로 UTF-8로 인코딩된 텍스트 데이터를 전송할 수 있다.