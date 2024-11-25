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
상위 프로토콜을 표기함으로써 Multiplexing과 DeMultiplexing이 일어난다. 
TCP로 보낼지, UDP로 보낼지를 헤더에 적어놓는다.

![[Pasted image 20241125164146.png|400]]

## example
### example 1
IP 패킷이 첫 번째 8비트가 다음과 같이 도착했습니다:
수신자는 패킷을 폐기합니다. 왜일까요?

해결책:
이 패킷에는 오류가 있습니다. 왼쪽에서부터 4비트(0100)는 버전을 나타내며, 이는 올바릅니다(IPv4이므로). 
다음 4비트(0010)는 잘못된 헤더 길이(2 × 4 = 8)를 나타냅니다. 
헤더의 최소 바이트 수는 20이어야 합니다. 
이 패킷은 전송 중 손상되었습니다.
### example 2
IP 패킷에서 HLEN 값이 이진수로 1000입니다. 이 패킷이 옵션으로 몇 바이트를 전송하고 있습니까?

해결책:
HLEN 값은 8입니다. 이는 헤더의 총 바이트 수가 8 × 4, 즉 32바이트임을 의미합니다. 첫 20바이트는 기본 헤더이고, 다음 12바이트는 옵션입니다.
### example 3
IP 패킷에서 HLEN 값이 5이고 총 길이 필드 값이 0028<sub>16</sub>입니다. 이 패킷이 전송하고 있는 데이터는 몇 바이트입니까?

해결책:
HLEN 값은 5입니다. 이는 헤더의 총 바이트 수가 5 × 4, 즉 20바이트(옵션 없음)임을 의미합니다. 
총 길이는 40바이트입니다. 
이는 패킷이 20바이트의 데이터를 전송하고 있음을 의미합니다(40 − 20).
### example 4
IP 패킷이 다음과 같은 16진수 숫자로 도착했습니다:
`45000028000100000102...`
이 패킷이 폐기되기 전까지 몇 홉을 이동할 수 있습니까?
이 데이터는 어떤 상위 레이어 프로토콜에 속합니까?

해결책:
4bit -> 1byte
TTL -> `01`(9번째 byte)
time-to-live 필드를 찾기 위해 우리는 8바이트(16진수 16자리)를 건너뜁니다. time-to-live 필드는 아홉 번째 바이트로, 이는 01입니다. 이는 패킷이 단 하나의 홉만 이동할 수 있음을 의미합니다. 프로토콜 필드는 다음 바이트(02)로, 이는 상위 레이어 프로토콜이 IGMP임을 의미합니다(표 7.2 참조). (프로토콜 번호는 몰라도 됨)
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
## Identification
패킷
## Detailed fragmentation example
![[Pasted image 20241125204803.png|400]]
맨 마지막 패킷은 More fragments를 0으로 해준다.
상황에 따라서 패킷의 경로가 달라질 수도 있는데, 분할이 또 되는 case가 있다면 또 분할된다. (example 참고)











