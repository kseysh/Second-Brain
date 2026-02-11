Simple Text Oriented Message Protocol로, 기존 Websocket 통신 방식을 좀 더 효율적이고 쉽게 다룰 수 있게 해주는 프로토콜

pub, sub의 개념으로 동작한다.

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