## ARP operation
![[Pasted image 20241204025111.png|500]]
IP 주소를 가지고 목적지 IP주소를 갖고 있는 기기에게 응답을 요청함 (broad cast 주소로 전송된다)
그러면 IP주소를 가진 System B는 응답으로 MAC주소를 응답한다.
## ARP packet
![[Pasted image 20241204025559.png|500]]
ARP -> IP주소를 주면 IP를 사용하는 MAC주소를 주는 protocol
RARP -> MAC주소를 주면 MAC 주소를 사용하는 IP주소를 주는 protocol
ARP request는 broadcast이고, ARP reply는 unicast이다.
## Encapsulation of ARP packet
![[Pasted image 20241204025202.png|500]]

## ARP가 필요한 4가지 상황
![[Pasted image 20241204132926.png]]
- Case 1
	- Sender와 Receiver가 같은 네트워크에 있는 상황
- Case 2
	- host가 패킷을 다른 네트워크에 전달하는 상황
- Case 3
	- router가 패킷을 다른 네트워크에 전달하는 상황
- Case 4
	- router가 같은 네트워크에 있는 host에게 패킷을 전달하는 상황
![[Pasted image 20241204133556.png|500]]
A가 B에게 패킷을 보내려는 상황
ARP Request를 보낼 때 MAC주소를 1로 채워 broadcast로 보낸다.
0x0806을 보고 ARP인 것을 확인하고 0x0001을 보고 ARP request인 것을 확인한다.
 ARP reply는 MAC 부소가 채워지고 unicast로 보내진다.
 0x0002를 보고 ARP reply인 것을 확인한다.

## ARP components
![[Pasted image 20241204134938.png|500]]
output module: ARP packet이 output으로 나오는 모듈
- IP packet을 보고 테이블 찾아봐서 resolved가 되면 IP packet을 보내준다.
- state가 F라면 P로 놓고 queue에 요청을 넣고 ARP request를 보낸다.
- state가 P라면 queue에만 넣어둔다.
input module: ARP packet을 input으로 받는 모듈
- ARP packet을 받아 Table 업데이트(P면 R로 R이면 time-out 업데이트)
- P -> R로 변한다면 queue에 있는 값을 모두 꺼내서 응답하는 모듈
### cache table
![[Pasted image 20241204135033.png|500]]
- State
	- R(Resolved): IP주소와 MAC주소가 매핑되어 있음
	- P(Pending): IP주소로 MAC주소를 알아보고 있는 상황
	- F(Free): 빈칸일 때