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

## ex
IP 주소가 130.23.43.20이고 물리적 주소가 B2:34:55:10:22:10인 호스트가 IP 주소 130.23.43.25이고 물리적 주소가 A4:6E:F4:59:83:AB인 다른 호스트로 패킷을 보내려고 합니다. 두 호스트는 같은 이더넷 네트워크에 있습니다. 이더넷 프레임에 캡슐화된 ARP 요청 및 응답 패킷을 보여주세요.

해결 방법

그림 8.6은 ARP 요청 및 응답 패킷을 보여줍니다. 이 경우 ARP 데이터 필드는 28바이트이고, 개별 주소가 4바이트 경계에 맞지 않는다는 점에 유의하십시오. 그래서 이러한 주소에 대해 일반적인 4바이트 경계를 표시하지 않습니다. 또한 IP 주소는 16진수로 표시됩니다.
![[Pasted image 20241204133556.png|500]]
A가 B에게 패킷을 보내려는 상황

