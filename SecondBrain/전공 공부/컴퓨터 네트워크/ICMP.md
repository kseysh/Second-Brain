# Internet Control Message Protocol Version 4
IP 프로토콜에는 오류 보고 또는 오류 수정 메커니즘이 없어 ICMP를 통해 오류를 보고한다.
ICMP는 항상 에러메시지를 original source에게 보고한다.

ICMP 메시지는 error reporting 메시지와 query 메시지로 나뉜다.
error reporting message: 라우터나 호스트(목적지)가 IP 패킷을 처리할 때 겪을 수 있는 문제를 보고
query message: 쿼리 메시지는 쌍으로 발생하며, 호스트나 네트워크 관리자가 라우터나 다른 호스트로부터 특정 정보를 얻는 데 도움을 준다.
## Position of ICMP in the network layer
![[Pasted image 20241127154718.png|500]]
![[Pasted image 20241127154832.png]]
type 번호 안 외워도 됨
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

## ?
![[Pasted image 20241130160302.png]]
16가지 종류 detact 가능 detail한 정보를 code에 들어간다
p12
p14
노트 잘 읽기
p16
라우터 버퍼가 꽉차 혼잡이 발생했을 때 보내는 양을 억제해달라는 목적으로 만들어짐
p17
혼잡때문에 라우터가 버릴 때 보내준다.
p19
라우터는 ttl이 0이면 패킷을 버리는데 icmp의 time-exceeded message를 보내서 알려준다. 
fragmentation된 패킷이 오지 않았을 때도 time-exceeded message를 보내서 알려준다,.
type number는 몰라도 code number는 알아두자
p23
checksum이나 ip주소가 이상하다 생각되면 original source에게 이상하다고 알림
p24
코드 두개인건 무시
몇번ㅉ에 오류가 있는지 알려주는 pointer값이 있다
p25
redirection concept
a가 b에게 보내려는 상황
default 라우터가 r1인 상황
a는 r2와 연결되어 있지만 굳이 r1을 거쳐서 갔음 
R1가 보내는 r2가 같은 네트워크이고, A도 같은 네트워크라는 것을 확인하면 A에게 b에게 보낼 때는 r2로 보내라고 알려준다.
p26
읽기
p27
targer router = r2로 설정
아래 하얀색 부분에는 
p28
읽기
