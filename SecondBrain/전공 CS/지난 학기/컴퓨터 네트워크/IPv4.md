Internet Protocol Version 4
## IP Diagram
![[Pasted image 20241125162536.png|400]]
### Service type
서비스 타입은 잘 쓰이지 않음(TCP 헤더의 Argent pointer처럼)
### Total length
![[Pasted image 20241125163428.png]]
 헤더와 데이터를 포함한 Datagram의 전체 길이
 만약 1byte의 데이터를 보낸다면, TCP 기본 헤더 20 IP 기본 헤더 20 총 41byte를 보내게 된다.
 그 후 frame도 헤더와 트레일러를 붙이게 되는데  처음 frame을 디자인할 때 frame의 payload가 최소 46byte가 되도록 디자인 하여 만약 41byte가 minimum이므로 이를 채우기 위해 Padding값이 들어가게 된다.
 상대방은 이 값을 받아 Padding만큼은 버려야하는데, 따라서 어느정도 길이까지가 표현해야 하는 값이다라는 것을 알려주기 위해서 Total Length가 필요하다.
16byte를 할당했으므로 IP datagram은 최대 65535byte까지 갈 수 있다.
### Time to live
만약 패킷이 loop로 인해 계속 돌게 되면 이는 시간이 지나면 사라지도록 해야한다.
이를 해결하기 위해 소멸되는 시간을 작성한다.
하지만 시간을 다루기가 복잡하여 maximum hop count를 작성해서 hop to hop을 할 때마다 1씩 감소시켜 반환한다.
만약 0이 되면 패킷을 전달하지 않고 버린다.
### Protocol
Transport layer에는 TCP와 UDP, Network layer에는 ICMP, IGMP, OSPF라는 protocol이 있는데, 
ICMP, IGMP, OSPF는 같은 network layer일지라도 IP헤더를 포함을 하고 전송되는 프로토콜이라 안에 표기를 한다.
상위 프로토콜을 표기함으로써 Multiplexing과 DeMultiplexing이 일어난다. 
TCP로 보낼지, UDP로 보낼지를 헤더에 적어놓는다.
![[Pasted image 20241125164146.png|400]]

## Fragmentation
![[Pasted image 20241125170826.png|400]]
Frame의 payload부분에 minimum 값이 존재하고 그게 46byte이다. MTU라고 불리는 maximum값도 존재를 하는데, 이는 physical network마다 다르다.
따라서 physical network마다 허용하는 IP datagram의 최대값이 다르다.
## Flags field
![[Pasted image 20241125171129.png|300]]
physical network마다 허용하는 IP datagram의 최대값이 다르므로 datagram이 분할 될 수도 있다.
D는 이것은 분할하지 말라는 의미이고, 그에 따라 전송되지 못하면 패킷을 버린다.
M은 분할 후 재조합할 때 사용하는 field이다. (마지막 패킷이 뭔지를 찾아보기 위해 사용하는 field)
## Fragmentation example
![[Pasted image 20241125171634.png|400]]
Fragmentation offset으로 분할한 것의 번호를 붙이는데, Fragmentation offset이 최대 13bit이므로 최대 8000정도까지 분할이 가능하다.
하지만 IP datagram의 max length는 65535bit이므로 최대 65535라는 번호를 붙일텐데, 13bit만으로는 원하는 만큼 분할하지 못할 수도 있다. 그래서 HLEN처럼 없어진 3bit를 위해 곱하기 8을 해준다.
Fragmentation에는 처음부터 얼마나 떨어져 있는지에 대한 값을 저장해둔다.
#### MTU
MTU는 header와 data가 포함된 값이다.
패킷이 분할된 상황에서 MTU를 추측해볼 수 있는데, 헤더 20 byte, 0000부터 1399까지 1400 byte이므로 MTU가 1420byte임을 추측할 수 있다.
## Detailed fragmentation example
![[Pasted image 20241125204803.png|400]]
맨 마지막 패킷은 More fragments를 0으로 해준다.
상황에 따라서 패킷의 경로가 달라질 수도 있는데, 분할이 또 되는 case가 있다면 또 분할된다. (example 참고)
## Option
![[Pasted image 20241125210251.png|500]]
Copy only in first fragment => 분리될 때 옵션을 첫 패킷에만 가지고 가고 나머지는 가져가지 않음

![[Pasted image 20241125210508.png|350]]
single-byte -> padding용 option (중요 x)
## Record-route option
![[Pasted image 20241125210714.png|400]]
## Record route concept
![[Pasted image 20241125211902.png|500]]
첫 번째에 있는 것은 첫 번째 라우터의 라우터 IP 주소, 두 번째에 있는 것은 두 번째 라우터의 라우터 IP 주소이다.
record route option을 이용해서 어느 라우터들을 거쳐왔는지 알 수 있다.
라우터는 최소 두개의 IP 주소를 갖는다. 왜냐하면 라우터는 왼쪽 네트워크의 멤버이기도 하고, 오른쪽 네트워크의 멤버이기도 하기 때문이다.
들어오는 쪽이 아닌 나가는 쪽의 라우터 주소를 갖는다. 그래야 돌아오면서 어떤 라우터를 지나왔는지 알 수 있다.
## Strict-source-route option
![[Pasted image 20241125212338.png|400]]

![[Pasted image 20241125212511.png|500]]
Sender가 패킷을 보내서 거쳐가야할 라우터의 순서를 미리 정해두는 것.
Destination과 IP address가 계속 바뀌면서 포인터 값이 4만큼 증가한다.
그 후 다음 번 라우터로 보내준다.
여기서는 라우터 이전의 라우터 주소를 사용한다. (어느 라우터로 가야하는지를 알아야하기 때문)
만약, 보내야 할 next가 연결되어 있지 않거나 유효하지 않으면 패킷을 버린다.
그래서 패킷이 잘 갔다면 반드시 경로대로 갔다는 것을 보장한다.
일반적인 사용자들보다는 네트워크 관리자가 사용한다 (어떤 경로로 보낼 수 있는지 테스트하기 위해 사용)
## Loose-source-route option - 개념만 알아도 됨
Strict-source-route-option에서는 보내야 할 next가 연결되어 있지 않거나 유효하지 않으면 패킷을 버리지만, Loose-source-route option에서는 순서가 약간 바뀌거나 중간에 다른 곳을 거쳐와도 패킷을 버리지 않는다.
## Time-stamp option - 크게 안 중요함
![[Pasted image 20241127151053.png|500]]
## Timestamp concept
![[Pasted image 20241127151137.png|500]]

## Reassembly table
![[Pasted image 20241127154456.png|500]]
재조합은 목적지 컴퓨터에서만 하기로 한다.
IP layer에서는 어떤 것이 없어져도 재전송 요구하지 않는다 (재전송 요구는 TCP 쪽에서 하기 때문이다.)
IP layer에서는 datagram이 없어지면 통째로 날라간다.
fragmentation 중 하나만 없어져도 tcp입장에서는 하나가 통째로 날라간 것이므로 통째로 날라가버린다. 복구에 걸리는 오버헤드가 커진다.

### segment를 크게 만들었을 때의 부작용
packet loss detact도 늦어지고 fragmentatioin 중 하나만 없어져도 tcp는 큰 하나가 통째로 날라간거라 tcp가 복구할 때 분리되지 않은 전체를 복구해야 한다. 따라서 복구에 걸리는 오버헤드가 커진다.