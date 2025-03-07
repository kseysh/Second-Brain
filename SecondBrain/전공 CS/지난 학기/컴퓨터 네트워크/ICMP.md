# Internet Control Message Protocol
IP 프로토콜에는 오류 보고 또는 오류 수정 메커니즘이 없어 ICMP를 통해 오류를 보고한다.
ICMP는 항상 에러메시지를 original source에게 보고한다.

ICMP 메시지는 error reporting 메시지와 query 메시지로 나뉜다.
error reporting message: 라우터나 호스트(목적지)가 IP 패킷을 처리할 때 겪을 수 있는 문제를 보고
query message: 쿼리 메시지는 쌍으로 발생하며, 호스트나 네트워크 관리자가 라우터나 다른 호스트로부터 특정 정보를 얻는 데 도움을 준다.
![[Pasted image 20241127154832.png]]
## ICMP messages 포맷
![[Pasted image 20241127154915.png|400]]
code: 한 타입 안에 여러가지 경우의 수를 나타낼 때 사용
## Error-reporting message 종류
![[Pasted image 20241127155151.png|550]]
## 에러 메시지를 위한 ICMP 데이터 필드
![[Pasted image 20241127154747.png|400]]
![[Pasted image 20241130160045.png|400]]
패킷을 받아 ICMP packet을 만든다.
tcp 앞 헤더 8byte(source/destination port num과 seq번호)를 가져온다.
무슨 에러가 생겼는지는 ICMP를 보고, 어디서 에러가 생겼는지는 첫번째 IP header, 어디로 가려했는지는 뒤 IP header를 본다.
8byte는 어떤 프로그램에서 오류가 발생했는지 알려준다(TCP 앞 4byte로 port번호를 볼 수 있으므로)(TCP앞 4-8byte에서 몇번째 seq에서 오류가 발생했는지 알려준다.)

## Destination-unreachable message format
![[Pasted image 20241130160302.png|500]]
16가지 종류 detact 가능 detail한 정보를 code에 들어간다
type number는 몰라도 code number는 알아두자
### Destination-unreachable code 종류
• code 0 - 네트워크 도달불가(Network Unreachable):
	• 목적지 네트워크로 가는 경로가 없음.
	• 목적지 주소가 라우팅 테이블에 없거나 디폴트 라우트가 없는 경우 발생.
• code 1 - 호스트 도달불가(Host Unreachable):
	• 최종 목적지 호스트에 도달할 수 없을 때 발생.
	• 호스트 또는 라우터에서 생성됨.
• code 2 - 프로토콜 도달불가(Protocol Unreachable):
	• 목적지 시스템에서 특정 프로토콜을 사용할 수 없다는 사실을 통보할 때 발생.
• code 3 - 포트 도달불가(Port Unreachable):
	• 목적지 호스트에서 특정 포트 번호를 사용할 수 없음을 알릴 때 발생.
• code 4 - 단편화 필요하지만 DF 설정됨(Fragmentation Required but DF bit is set):
	• IP 데이터그램이 MTU가 작은 네트워크를 통과하려면 단편화가 필요하지만, DF 비트가 설정된 경우 라우터는 이를 폐기하고 송신측에 이를 통보.
• code 13 - 목적지와의 통신이 관리적으로 금지됨(Communication Administratively Prohibited):
	• 어떤 이유로든 목적지가 통신을 원하지 않을 때 발생.
	• 예를 들어, 방화벽이 운용 정책에 위배되는 데이터그램을 의도적으로 폐기할 경우. 이 오류 메시지를 원천지에 보낼 수도 있고 보내지 않을 수도 있음.

code 2, 3은 목적지 호스트가 생성하며 나머지는 라우터가 생성한다.
라우터는 패킷 전송을 막는 모든 문제를 감지할 수 없다.
IP protocol은 flow-control과 congestion control이 없다.
## Source-quench
![[Pasted image 20241130162422.png|500]]
라우터 버퍼가 꽉차 혼잡이 발생했을 때 보내는 양을 억제해달라는 목적으로 만들어짐
Source-quench message는 혼잡때문에 라우터가 버릴 때 보내준다.
혼잡으로 인해 datagram이 폐기될 때마다 하나의 Source-quench message가 전송된다.
## Time-exceeded message
![[Pasted image 20241130162858.png|500]]
라우터는 ttl이 0이면 패킷을 버리는데 icmp의 time-exceeded message를 보내서 알려준다. 
fragmentation된 패킷이 오지 않았을 때도 time-exceeded message를 보내서 알려준다.

code 0: 시간 제한 필드의 값이 0임을 나타내기 위해 라우터에서만 사용
code 1: 설정된 시간 내에 모든 조각이 도착하지 않았음을 나타내기 위해 목적지 호스트에서만 사용
## Parameter-problem message
![[Pasted image 20241130163136.png|500]]
checksum이나 ip주소가 이상하다 생각되면 original source에게 이상하다고 알릴 때 사용
코드 두개인건 무시
몇번째에 오류가 있는지 알려주는 pointer값이 있다
## Redirection concept
![[Pasted image 20241130163538.png|500]]
a가 b에게 보내려는 상황
default 라우터가 r1인 상황
a는 r2와 연결되어 있지만 굳이 r1을 거쳐서 갔음
R1가 보내는 r2가 같은 네트워크이고, A도 같은 네트워크라는 것을 확인하면 A에게 b에게 보낼 때는 r2로 보내라고 알려준다.
## Redirection message
호스트는 일반적으로 작은 라우팅 테이블로 시작하여 점차적으로 확장되고 업데이트됩니다. 이를 수행하기 위한 도구 중 하나가 리디렉션 메시지이다.
![[Pasted image 20241130164037.png|400]]
targer router = r2로 설정
redirection message는 라우터로부터 같은 로컬 네트워크에 있는 호스트로 보내진다.
## Echo-request message
![[Pasted image 20241130165237.png|400]]
echo-request message는 호스트나 라우터가 보낼 수 있다
echo-reply message는 echo-request message를 받는 메시지가 응답하는 것이다.
상대방 컴퓨터에 IP 주소가 할당되어 있어 네트워크가 연결되어 있는지를 확인하기 위해 사용한다.
보통 ping command는 이 echo-request message를 이용해 구현한다.
Echo-request message는 보통 RTT를 측정할 때 자주 쓰인다.
## Timestamp message
![[Pasted image 20241213023101.png|300]]
timestamp-request message에서 original timestamp를 적어서 보내고, timestamp-reply message에서는 receive timestamp와 transmit timestamp를 적어서 보낸다.
보낸 시간과 도착시간의 차에서 transmit timestamp와 reveive timestamp의 차이를 빼주면 RTT를 보다 더 정확하게 알 수 있다.
상대방과 자신의 시계가 다르더라도 정확하게 RTT를 측정할 수 있다. 또한 시계의 sync를 맞출 수도 있다.
## traceroute program operation
traceroute는 대부분 ICMP를 가지고 구현한다.
![[Pasted image 20241130172345.png|500]]

![[Pasted image 20241130172620.png|500]]
host A는 destination B, TTL=1로 datagram을 보내면 R1에서 ICMP 메시지를 다시 A에게 보낸다. 이로 인해 R1까지 가는 경로와 시간을 알게 되고, 그 후에는 TTL=2로 보내고 목적지에 도착할 때까지 이런 방식으로 traceroute를 ICMP로 구현한다.
대부분의 학교망이나 회사망은 외부에서 들어오는 ICMP는 방화벽에서 막는다. 따라서 traceroute를 해도 * * * 만 뜰 수도 있다.
