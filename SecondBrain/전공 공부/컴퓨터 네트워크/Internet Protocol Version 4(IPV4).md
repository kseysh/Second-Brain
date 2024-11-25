## IP Diagram
![[Pasted image 20241125162536.png|400]]
### Service type
![[Pasted image 20241125162553.png|400]]
![[Pasted image 20241125162609.png|400]]
서비스 타입은 잘 쓰이지 않음(TCP 헤더의 Argent pointer처럼)
### Total length
![[Pasted image 20241125163428.png]]
 헤더와 데이터를 포함한 Datagram의 전체 길이
 만약 1byte의 데이터를 보낸다면, TCP 기본 헤더 20 IP 기본 헤더 20 총 41byte를 보내게 된다.
 그 후 frame도 헤더와 트레일러를 붙이게 되는데  처음 frame을 디자인할 때 frame의 payload가 최소 46byte가 되도록 디자인 하여 만약 41byte가 minimum이므로 이를 채우기 위해 Padding값이 들어가게 된다.
 상대방은 이 값을 받아 Padding만큼은 버려야하는데, 따라서 어느정도 길이까지가 표현해야 하는 값이다라는 것을 알려주기 위해서 Total Length가 필요하다.
16byte를 할당했으므로 IP datagram은 최대 65535byte까지 갈 수 있다.
### Time to live
만약 패킷이 roop로 인해 계속 돌게 되면 이는 시간이 지나면 사라지도록 해야한다.
이를 해결하기 위해 소멸되는 시간을 작성한다.
하지만 시간을 다루기가 복잡하여 maximum hop count를 작성해서 hop to hop을 할 때마다 1씩 감소시켜 반환한다.
만약 0이 되면 패킷을 전달하지 않고 버린다.
### Protocol
Transport layer에는 TCP와 UDP, Network layer에는 ICMP, IGMP, OSPF라는 protocol이 있는데, 
ICMP, IGMP, OSPF는 같은 network layer일지라도 IP헤더를 포함을 하고 전송되는 프로토콜이라 안에 표기를 한다.
상위 프로토콜을 표기함으로써 Multiplexing은 여러개가 공유가 되는 것이고?
반대쪽에서는 DeMultiplexing이 일어난다. 
TCP로 보낼지, UDP로 보낼지를 헤더에 적어놓는다.

![[Pasted image 20241125164146.png|400]]




