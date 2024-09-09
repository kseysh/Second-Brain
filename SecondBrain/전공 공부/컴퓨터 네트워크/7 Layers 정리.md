## internet vs Internet
### internet
통신 기능을 갖춘 2개 이상의 장치의 집합
### Internet
TCP/IP 인터넷 프로토콜 모음을 사용하여 구축된 컴퓨터 네트워크 (WWW)

## ISO vs OSI
### ISO
International Standards Organization (조직)
### OSI
Open Systems Interconnection (모델)

# 7 Layer
![[Pasted image 20240909144311.png|300]]
# 7 Layer 비유
![[Pasted image 20240909144236.png|350]]
- 이사장님 (Application Layer) 
	- ex) email, http
- 비서 (TCP) 
	- 패킷이 없어졌을 때 복구를 수행함, 패킷마다 번호를 붙임
- 배송 회사 (Network Layer) 
	- 주소를 입력함 (IP를 이용함)
	- IP Header를 만든다.
- Data Link Layer
	- 내가 보내려는 목적지에 따라 물류회사가 달라질 수가 있고, 지정된 물류회사로 갈 수도 있다 
	  (옵션은 여러 개다.)
	- ex) 미추홀구로 보내는 택배는 미추홀구 물류 회사, 부평구로 보내는 택배는 부평구 물류 회사로 모아놓는다. (어디로 보내는 지에 따라 어디 물류 회사로 보낼 지 정한다.)

#### hop to hop (mac 주소)
네트워크가 step by step으로 이동하는 것 
로컬에서는 mac주소를 이용해 패킷이 이동한다.
#### source to destination (IP 주소)\
출발지 및 목적지를 패킷에 작성할 때는, IP주소를 활용한다.

## Packet Switching Network (목적지 주소 전달 방식)
출발 IP와 목적지 IP만 가지고 출발한다. 각 교차로마다 목적지 주소를 보고 어디로 가라고 알려준다. 
일부 경로가 죽더라도 다른 경로로 가면 되기 때문에 보안적으로 유리하다.

### Circuit Switching Network (중앙제어 전달 방식)과의 차이
우리가 사용하는 Internet이 아닌 방식이다.
중앙서버에서 출발지부터 목적지까지 가기 위해 어떻게 이동해야하는지 그 패킷만의 경로를 알려준다.
하지만, 네트워크는 전쟁 상황에서 출발하였기 때문에 중앙 서버가 죽으면 모든 서버가 죽으므로 보안적으로 유리하지 않다.

# OSI 7계층 요약
![[Pasted image 20240909145314.png|400]]
### Application, Presentation, Session Layer
네트워크 자원에 대한 접근을 허용함
### Transport Layer
port 관리
받은 패킷을 어느 창에 띄워야 할지 정해준다. (process to process 커뮤니케이션)
프로세스 간 신뢰할 수 있는 메시지 전달과 오류 복구를 제공한다.
### Network Layer
IP 주소(Logical addresses) 관리
패킷을 출발지에서 목적지까지 이동시키며, 인터넷워킹을 제공한다.
### Data Link Layer
Mac 주소(Physical adresses) 관리
비트를 프레임으로 구성하며, hop to hop 전달을 제공한다.
### Physical Layer
비트들을 어떻게 전송할지에 대한 책임을 지닌다.

# 각 계층별 커뮤니케이션 단위
- Application, Presentation, Session Layer
- Transport Layer
- Network Layer
-  Data Link Layer
	- frame (data)
- Physical Layer
	- bit


# Physical Layer
비트들을 어떻게 전송할지에 대한 책임을 지니는 Layer이다. (ex) 유선, 무선, 와이파이, LTE, 3G 다 포맷이 다름 이 주파수 변환 및 데이터 통신을 담당한다.)
라우터끼리 비트를 어떻게 전송할지 결정한다.
커뮤니케이션 단위: bit
![[Pasted image 20240909150745.png|400]]
# Data Link Layer
