Simple Text Oriented Message Protocol로, 기존 Websocket 통신 방식을 좀 더 효율적이고 쉽게 다룰 수 있게 해주는 프로토콜

pub, sub의 개념으로 동작한다.

클라이언트와 서버가 전송할 메시지 유형, 형식, 내용을 정의한다.

Frame 기반의 프로토콜이며, 아래와 같은 형식을 가진다

```
COMMAND
header1:value1
header2:value2

Body...^@
```

클라이언트는 Message를 전송하기 위해 SEND, SUBSCRIBE, UNSUBSCRIBE, COMMAND를 사용할 수 있다.

![[Pasted image 20260210212909.png]]


Spring에서는 WebSocketConfigurer와 다르게 WebSocketMessageBrokerConfigurer를 사용한다.
WebSocketConfigurer는 순수 웹소켓을 다루기 위한 설정이다.

## WebSocketMessageBrokerConfigurer
STOMP 프로토콜을 사용하여 메시지 브로커 기능을 활용하기 위한 설정
=> 현재 프로젝트에서 Redis Pub/Sub을 활용하기 위한 설정으로 볼 수도 있다.
@MessageMapping, @SendTo 같은 어노테이션을 사용하여 컨트롤러 형식으로 메시지를 처리할 수 있다.

## STOMP의 인증 단계
보통 JWT는 Authorization 헤더에 담는 것이 표준인데, 순수 웹소켓 자바스크립트의 WebSocket은 헤더 변경을 지원하지 않음
따라서 핸드셰이크는 들어올 수 있게 열어두고, 연결된 직후에 첫 번째 STOMP 프레임 헤더에 토큰을 실어서 인증한다.

## 유저가 첨삭 링크로 들어올 때, 서버에서 처리해야할 플로우
1. HTTP 요청: 링크 유효성 확인 (`/api/v1/share-link/{shareLinkId}/permission`)
2. STOMP 연결: 인증 및 리뷰어 정원 초과 확인
	1. ChannelInterceptor(preSend method) 사용자 인증 구분
	2. Frame 헤더에서 Authorization과 shareId 추출
	3. Jwt에서 userId를 뽑아 Role 부여 (writer, reviewer)
3. 구독: 해당 방 listening
4. 입장 알림: 현재 접속자 상태 알림 및 데이터 동기화
5. Reviewer 퇴장 처리: SessionDisconnectEvent 리스너를 구현하여, Redis에서 자리를 비워주기

## STOMP Interceptor
- `HandshakeInterceptor`: 웹소켓 연결이 맺어지기 전, HTTP 레벨에서 가로채는 역할
	- `ws://` 역할이 들어왔을 때, TCP 연결을 맺기 직전에 실행. 아직 STOMP 프로토콜이 시작되지 않은 순수 HTTP 상태
- `ChannelInterceptor`: 웹소켓 연결 후, STOMP 메시지 레벨에서의 인터셉터
	- 클라이언트가 주고받는 실제 STOMP 프레임 (CONNECT, SEND, SUBSCRIBE)을 가로챈다.
## in 배민..
커맨드가 많을수록 코드상에서의 분기가 늘어나고 다양한 요청들을 처리해야 하기 때문에 복잡성이 늘어난다.
REST API로 가능한 것들이 소켓 커맨드로 들어오는 일을 막는다.
메시지를 보내고 받는 본질만 남겨둔다.

## STOMP Controller 코드
@MessageMapping: 이 주소로 발행된 메시지를 (prefix를 제외한 path)
@SendTo: 이 주소를 구독한 사용자에게 전달

## STOMP 예외 처리
@MessageExceptionHandler