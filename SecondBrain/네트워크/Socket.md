# 프로토콜 표준에서 정의한 socket
IP와 port number를 합친 개념으로 인터넷 상에 존재하는 각 port를 유니크하게 식별하기 위한 주소
각 socket은 인터넷 상에서 유니크하다.

socket은 각 [[connection]]을 유니크하게 식별할 수 있어야 한다.
=> 한 쌍의 socket은 connection을 유니크하게 식별한다.

하나의 socket은 동시에 여러 connection들에서 사용될 수 있다.
![[Pasted image 20240916023530.png]]
### UDP는?
connectionless: 연결을 맺지 않고 바로 데이터를 주고 받는다.]
unreliable: internet protocol을 거의 그대로 사용 (tcp가 아니므로)

# 네트워크 프로그래밍에서의 socket
### TCP/IP stack
![[Pasted image 20240916024159.png|300]]
TCP/IP stack은 아래와 같이 구현될 수 있다.
![[Pasted image 20240916024253.png|300]]
## Socket
- 애플리케이션이 시스템의 기능을 함부로 쓸 수는 없다
	- 대신 시스템은 애플리케이션이 네트워크 기능을 사용할 수 있도록 프로그래밍 인터페이스를 제공한다
- 애플리케이션은 socket을 통해 데이터를 주고 받는다
	- 개발자는 socket programming을 통해 네트워크 상의 다른 프로세스와 데이터를 주고 받을 수 있도록 구현한다.
- 대부분의 시스템은 socket 형태로 네트워크 기능을 제공한다.
- 보통 socket을 직접 조작해서 통신 기능을 구현할 일은 적다.
	- application layer의 프로토콜은 보통 라이브러리나 모듈 형태로 해당 기능이 제공되는데, 이 때 내부를 열어보면 소켓을 활용해서 프로토콜을 구현했음을 알 수 있다.
- 

