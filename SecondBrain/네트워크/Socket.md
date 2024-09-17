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

### 프로토콜 표준에서 정의한 것 처럼 socket은 \<protocol, IP, port>로 유니크하게 식별되는가?
UDP는 맞지만, TCP는 그렇지 않다.

#### TCP socket 동작 방식
서버는 클라이언트가 connection 맺는 요청을 기다리는 listening socket이 있어야 한다.
client가 서버에게 connection을 맺자는 요청이 오면, 3-way-handshake를 통해 서버와 connection을 맺는다.
connection이 성립되면 서버는 다른 socket을 만든다. 
client는 서버가 만든 새로운 socket과 통신한다.

다른 client가 오면 listening socket을 통해 connectino을 맺고, 서버는 또 다른 socket을 만들어 다른 client와 통신한다.
![[Pasted image 20240916025459.png|400]]
#### 그렇다면, 어떻게 socket을 식별할까?
connection 연결 요청: listening socket으로 요청한다.
conneciton이 성립된 이후: \<src IP, src port, dest IP, dest port>로 socket을 식별한다. (세 개의 TCP socket이 dest IP address와 dest port number가 동일하므로 src IP와 src port로 socket을 식별한다.)
#### 클라이언트 쪽에서도 같은 IP와 port를 가지는 서로 다른 TCP 소켓이 생길 수 있다.
![[Pasted image 20240916030447.png|300]]
클라이언트에서는 OS level에서 port와 IP를 bind해주는데, 이는 사용되지 않는 port number를 확인하여 bind 해준다.
그래서 사용되지 않는 port number가 없다면 client의 서로 다른 socket이 동일한 IP와 port를 가질 수 있다.
이 때 서버에 connection을 맺자는 요청을 같은 서버에 또 보내게 되면 그 connection은 이루어지지 않도록 TCP 스펙상 만들어져있다.
### UDP 소켓은 프로토콜 표준에 맞게 IP와 port가 unique하다.
![[Pasted image 20240916030405.png|300]]
- UDP socket에서 데이터를 보낼 때 어느 UDP socket으로 보낼지 지정할 수 있다.
- UDP socket에서 데이터를 읽을 때 어느 UDP socket으로부터 왔는지 알 수 있다.
