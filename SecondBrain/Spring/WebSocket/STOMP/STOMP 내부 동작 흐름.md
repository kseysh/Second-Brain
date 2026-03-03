Spring STOMP 아키텍처는 클라이언트, 컨트롤러, 브로커 사이에서 메시지를 주고받기 위해 내부적으로 세 가지 주요 **메시지 채널(Message Channel)**을 생성합니다.
- **`clientInboundChannel`:** 클라이언트가 서버로 보낸 메시지(SEND, SUBSCRIBE 등)가 들어오는 통로입니다.
- **`clientOutboundChannel`:** 서버가 클라이언트로 메시지(MESSAGE)를 보낼 때 나가는 통로입니다.
- **`brokerChannel`:** 서버 내부의 컨트롤러(`@MessageMapping`)가 처리한 메시지를 메시지 브로커로 전달할 때 사용하는 통로입니다.

![[Pasted image 20260225162643.png]]

## 메시지 브로커의 종류
Spring은 기본적으로 **Simple In-Memory Broker**를 제공한다. 
이는 스프링 애플리케이션의 메모리 내에서 동작하므로 설정이 매우 간단하지만, 서버가 여러 대인 분산 환경에서는 서버 간 세션 공유가 되지 않는 한계가 있습니다.

이러한 한계를 극복하기 위해 Spring은 **External Broker(외부 브로커)** 연동을 지원합니다.
- **STOMP Broker Relay:** RabbitMQ, ActiveMQ와 같은 전문적인 메시지 브로커를 외부에 두고 연결합니다. 브로커 채널로 들어온 메시지를 외부 브로커로 넘기고(Relay), 외부 브로커가 클라이언트들에게 메시지를 분배하는 역할을 맡게 됩니다.