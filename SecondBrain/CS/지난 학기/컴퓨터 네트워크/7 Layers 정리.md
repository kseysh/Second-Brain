실습 문제는 쉽게 나옴
퀴즈는 하위 3개 제외
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
#### source to destination (IP 주소)
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
ISO에서 7Layer를 제안했지만, Internet은 5Layer고, 살아남은 것은 5Layer라서 5Layer로 이해하는 것이 편하다.
### Transport Layer
port 관리
받은 패킷을 어느 창에 띄워야 할지 정해준다. (process to process 커뮤니케이션)
### Network Layer
IP 주소(Logical addresses) 관리
패킷을 출발지에서 목적지까지 이동시키며, 경로 만들기에 관련한 일을 담당한다.
### Data Link Layer
Mac 주소(Physical adresses) 관리
비트를 프레임으로 구성하며, hop to hop 전달을 제공한다.
### Physical Layer
비트들을 어떻게 전송할지에 대한 책임을 지닌다.
라우터끼리 비트를 어떻게 전송할지 결정한다.

# 각 계층별 커뮤니케이션 단위 (패킷을 어떻게 명명하는지)

- Application, Presentation, Session Layer
	- `message`
- Transport Layer - port
	- `segment` (TCP에서의 패킷)
	- `userdatagram` (udp에서의 패킷)
	- `packet` (전체적인 transport layer에서의 패킷)
- Network Layer - ip
	- `Datagram`
-  Data Link Layer - mac
	- `frame`
- Physical Layer
	- `bit`

## Data Link Layer (physical addresses) (mac 주소)
![[Pasted image 20240909152123.png|550]]
### 10이 87에게 frame을 처음 보낼 때
1 => switch로 패킷을 보냄
2,3 => 자신의 것이 아니어서 버림
4 => 확인해보니 자신의 것이어서 가진다.
#### 필터링 기능
10이 87에게 frame을 보내고, 87이 10에게 다시 frame을 보낼 때는 10이 87에게 보냈던 것 처럼 28,53에게도 보내지 않는다.
필터링 기능을 이용해서 switch가 어디서 온 frame인지 저장해두고 이를 다시 보낼 때 이 저장된 정보를 이용해 frame을 보낸다.

## 리피터의 종류
### Repeater
1 대 1로 리피터와 리피터가 패킷을 통신한다.
신호가 약해지거나 손상되기 전에 신호를 재생성하여 네트워크의 길이를 확장한다.
![[Pasted image 20240909152610.png|400]]
### Hub
1 대 다로 허브와 허브들이 패킷을 통신한다. (1대n이 가능한 멀티포트 리피터)
![[Pasted image 20240909152653.png|200]]
### Bridge
필터링 기능이 있는 리피터
데이터 링크 계층에서 작동
### Switch
필터링 기능이 있는 허브 (필터링 기능이 있는 멀티포트 리피터)
데이터 링크 계층에서 작동

# Network Layer(logical addresses)의 Exemple
![[Pasted image 20240909153120.png]]
physical address인 MAC주소는 hop to hop (각 hop은 라우터 또는 스위치)으로 이동할 때마다 출발지와 목적지의 MAC 주소가 변경된다.
하지만, logical adderess인 IP주소는 변경되지 않는다.
### Mac 헤더와 IP 헤더
Mac 헤더는 도착 Mac 출발 Mac 순서이고,
IP 헤더는 출발 IP 도착 IP 순서이다.
이게 지워져 있어도 채울 수 있어야 할 듯.

