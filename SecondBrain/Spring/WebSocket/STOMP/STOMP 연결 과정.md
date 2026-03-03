## 웹소켓 연결 과정
![[Pasted image 20260303165352.png|400]]
`Connection: Upgrade` & `Upgrade: websocket` 헤더를 가진 Request 전송
`HTTP/1.1 101 Switching Protocols` 헤더를 가진 Response 수신

## STOMP 연결 과정
`CONNECT` FRAME 전송 및 `CONNECTED` 프레임을 응답으로 받음 이 때, 하트비트를 서로 주고 받는다.




