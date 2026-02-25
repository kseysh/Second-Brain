Spring STOMP 아키텍처는 클라이언트, 컨트롤러, 브로커 사이에서 메시지를 주고받기 위해 내부적으로 세 가지 주요 **메시지 채널(Message Channel)**을 생성합니다.
- **`clientInboundChannel`:** 클라이언트가 서버로 보낸 메시지(SEND, SUBSCRIBE 등)가 들어오는 통로입니다.
- **`clientOutboundChannel`:** 서버가 클라이언트로 메시지(MESSAGE)를 보낼 때 나가는 통로입니다.
- **`brokerChannel`:** 서버 내부의 컨트롤러(`@MessageMapping`)가 처리한 메시지를 메시지 브로커로 전달할 때 사용하는 통로입니다.

![[Pasted image 20260225162643.png]]

