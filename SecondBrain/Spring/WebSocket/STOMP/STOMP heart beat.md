형식: `{발신자가 수행할 수 있는 하트비트 간격},{수신자가 하트비트를 수행하고 싶은 간격}`
즉, 나,너

0은 HeartBeat를 보낼 수 없음 / HeartBeat를 수신하고 싶지 않다는 의미이다.

heart-beat 헤더가 없다면, `heart-beat:0,0`와 동일하게 처리한다.

heart-beat는 CONNECT, CONNECTED 프레임에서 주고 받는다.

## 하트비트 협상
STOMP 명세에 따라, 양방향 하트비트는 각각 더 큰 값으로 결정된다.