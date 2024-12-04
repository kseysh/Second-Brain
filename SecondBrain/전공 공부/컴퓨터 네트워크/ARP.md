## ARP operation
![[Pasted image 20241204025111.png|500]]
IP 주소를 가지고 목적지 IP주소를 갖고 있는 기기에게 응답을 요청함(broad cast 주소로 전송된다)
그러면 IP주소를 가진 System B는 응답으로 MAC주소를 응답한다.
## ARP packet
![[Pasted image 20241204025559.png|500]]
ARP -> IP주소를 주면 IP를 사용하는 MAC주소를 주는 protocol
RARP -> MAC주소를 주면 MAC 주소를 사용하는 IP주소를 주는 protocol
ARP request는 broadcast이고, ARP reply는 unicast이다.
## Encapsulation of ARP packet
![[Pasted image 20241204025202.png|500]]

11:57 까지 봄