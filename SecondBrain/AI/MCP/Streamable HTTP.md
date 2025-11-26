## Streamable HTTP 
HTTP POST/GET 요청으로 여러 클라이언트 연결 처리
전통적인 HTTP 통신을 확장하여 스트리밍처럼 작동하도록 만든 방식

![[Pasted image 20251121153123.png]]

## 사용 방법
#### Client가 Server로 메시지 전송 (HTTP POST)
클라이언트가 서버에서 받은 URI로 HTTP POST 요청을 보내며, 요청의 본문에는 JSON 형식의 데이터가 포함된다.
#### Server가 Client로 메시지 응답하기 (HTTP event-stream)
서버가 클라이언트에게 응답하기 위해 SSE 스트림을 통해 message 이벤트를 보내며, data 필드에는 응답 메시지가 JSON 형식으로 포함된다.
이 때 데이터의 응답은 스트리밍으로 아래와 같이 반환된다.

```json
HTTP/1.1 200 OK
Content-Type: text/event-stream
Transfer-Encoding: chunked

data: { "jsonrpc": "2.0", "id": "1", "messageId": "msg-101", "result": "오늘 날씨는..." }
data: { "jsonrpc": "2.0", "id": "1", "messageId": "msg-102", "result": "...맑고 따뜻합니다." }

event: end
data: [DONE]
```


```json
[T=0초] data: { "jsonrpc": "2.0", "id": "1", "messageId": "msg-101", "result": "오늘 날씨는..." }
[T=0.3초] data: { "jsonrpc": "2.0", "id": "1", "messageId": "msg-102", "result": "...맑고 따뜻합니다." }
[T=0.5초] event: end
[T=0.5초] data: [DONE]
```

## HTTP + SSE 방식과 다른점
Streamable HTTP가 연결이 끊겼을 때를 고려하여 messageId를 추가하여, 이어받기를 고려할 수 있다.
또한, 일반 HTTP처럼 짧은 연결로 구성가능하며, 필요 시 동적으로 SSE로 업그레이드할 수 있다.
# Stateless Streamable-HTTP
요청 간 세션 상태를 유지하지 않아 MSA 환경에 적합
SSE나 Streamable-HTTP는 서버 배포나 연결 끊김시 세션이 유실되어 사용에 불편할 수 있음