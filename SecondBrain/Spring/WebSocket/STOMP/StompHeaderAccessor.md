StompHeaderAccessor는 서버의 힙 메모리 내부에 있는 WebSocketSession 객체에 존재한다.

## Accessor가 웹소켓 데이터를 가져오는 구조
1. Spring이 관리하는 WebSocketSession 객체에 저장된다.
	1. 클라이언트가 처음 HandShake를 할 때 생성된다.
	2. WAS의 Heap에 ConcurrentHashMap 형태로 존재한다.
2. 클라이언트가 메시지를 보내거나, 서버가 메시지를 받을 때 Spring이 이 데이터를 Message라는 객체로 포장하여 Message 객체의 Header 부분에 집어넣는다. (simpSessionAttributes)
3. 접근 도구: StompHeaderAccesor