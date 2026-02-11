Simple Text Oriented Message Protocol로, 기존 Websocket 통신 방식을 좀 더 효율적이고 쉽게 다룰 수 있게 해주는 프로토콜

pub, sub의 개념으로 동작한다.

![[Pasted image 20260210212909.png]]


Spring에서는 WebSocketConfigurer와 다르게 WebSocketMessageBrokerConfigurer를 사용한다.
WebSocketConfigurer는 순수 웹소켓을 다루기 위한 설정이다.

## WebSocketMessageBrokerConfigurer
STOMP 프로토콜을 사용하여 메시지 브로커 기능을 활용하기 위한 설정
=> 현재 프로젝트에서 Redis Pub/Sub을 활용하기 위한 설정으로 볼 수도 있다.
@MessageMapping, @SendTo 같은 어노테이션을 사용하여 컨트롤러 형식으로 메시지를 처리할 수 있다.

