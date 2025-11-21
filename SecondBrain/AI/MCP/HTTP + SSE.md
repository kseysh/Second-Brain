## HTTP + SSE
Server Sent Events를 의미하며 서버가 실시간 업데이터를 클라이언트에 전송할 수 있는 HTTP 서버 푸시 기술
![[Pasted image 20251001214736.png|300]]
### 사용 이유
HTTP는 자주 연결을 설정해야 하므로 오버헤드가 크고 latency가 높지만, MCP는 지속적이고 latency가 낮은 데이터 스트림 필요
MCP는 양방향 메시지 통신을 위해 Server -> Client의 경우 SSE, Client -> Server 는 HTTP POST를 사용한다.

## 사용 방법
1. 클라이언트가 SSE 스트림 연결을 수립하기 위해 MCP 서버의 SSE 엔드포인트로 HTTP GET 요청을 보낸다.
	1. 이 요청이 성공하면 클라이언트는 서버로부터 지속적으로 메시지를 받는 통로를 열어둔 상태가 되며, 이 연결은 일반 HTTP와 달리 끊기지 않음. 즉, 서버는 연결을 유지한 채로, text/event-stream 포맷으로 지속적인 이벤트 스트림을 클라이언트에 보냄
2. Server가 Client로 Endpo