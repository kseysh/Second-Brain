
## stdio
MCP 서버가 stdin을 통해 JSON-RPC 메시지를 수신하고 stdout을 통해 응답을 작성한다.
프로세스 내부에서 표준 입출력으로 통신한다.
## SSE
Server Sent Events를 의미하며 서버가 실시간 업데이터를 클라이언트에 전송할 수 있는 HTTP 서버 푸시 기술
![[Pasted image 20251001214736.png|300]]
## Streamable HTTP 
HTTP POST/GET 요청으로 여러 클라이언트 연결 처리
## Stateless Streamable-HTTP
요청 간 세션 상태를 유지하지 않아 MSA 환경에 적합
SSE나 Streamable-HTTP는 서버 배포나 연결 끊김시 세션이 유실되어 사용에 불편할 수 있음
