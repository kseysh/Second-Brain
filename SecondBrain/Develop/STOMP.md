Simple Text Oriented Message Protocol로, 기존 Websocket 통신 방식을 좀 더 효율적이고 쉽게 다룰 수 있게 해주는 프로토콜

pub, sub의 개념으로 동작한다.

![[Pasted image 20260210212909.png]]


Spring에서는 WebSocketConfigurer와 다르게 WebSocketMessageBrokerConfigurer를 사용한다.
WebSocketConfigurer는 순수 웹소켓을 다루기 위한 설정이다.
