## Streamable HTTP 
HTTP POST/GET 요청으로 여러 클라이언트 연결 처리
전통적인 HTTP 통신을 확장하여 스트리밍처럼 작동하도록 만든 방식

![[Pasted image 20251121153123.png]]

## 사용 방법
#### Client가 Server로 메시지 전송 (HTTP POST)
클라이언트가 서버에서 받은 URI로 HTTP POST 요청을 보내며, 요청의 본문에는 JSON 형식의 데이터가 포함된다.
#### Server가 Client로 메시지 응답하기 (HTTP event-stream)


# Stateless Streamable-HTTP
요청 간 세션 상태를 유지하지 않아 MSA 환경에 적합
SSE나 Streamable-HTTP는 서버 배포나 연결 끊김시 세션이 유실되어 사용에 불편할 수 있음