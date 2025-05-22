1. readme.txt
2. 3
3. b2b.tech.support@intel.com
4. 
## Packet Sniffing
![[Pasted image 20250522103635.png|400]]
- Passive attack 의 일종 (Traffic analysis)
	- 네트워크 트래픽을 도청하고 민감한 정보를 수집하는 행위 (= eavesdropping)
- 노출된 ID, password, cookie 등을 엿볼 수 있는 위험
	- 평문 프로토콜: HTTP, TELNET, FTP
	- e.g. HTTP Cookie
 		- session hijacking: 쿠키에 담겨있는 로그인 세션 값 탈취 위험 있음
- 다운로드 불가능한 파일을 덤프(dump)하거나 엿볼 수 있는 위험
	- 네트워크를 통해 송수신되는 모든 파일
	- 개인 파일 ( 사진, 비디오 등 )
	- 수집된 패킷으로부터 파일을 복구할 위험 있음
- 스위치/라우터로 분리된 네트워크들 간에는 sniffing 어려움
	- A공유기 구역 ~ B공유기 구역
	- A공유기 구역의 x.2~x.3 의 통신을 B공유기 구역의 x.3이 sniffing 불가
![[Pasted image 20250522103816.png|400]]

## Wireshark
세계에서 가장 널리 쓰이는 네트워크 패킷 분석기
패킷 캡쳐를 위한 pcap 네트워크 라이브러리 사용
### Promiscuous mode
![[Pasted image 20250522104336.png|300]]
![[Pasted image 20250522104421.png|300]]
- 정상적인 네트워크 카드는 네트워크 카드에 인식된 2계층과 3계층 정보가 자신의 것과 일치하지 않는 패킷은 무시한다. 
- 정상적인 경우에는 이러한 Packet들을 처리할 이유가 없기 때문. 모든 Packet을 일일이 CPU가 확인하여 처리하면 CPU에 부하가 많아짐
- 그러나, 네트워크 카드의 모드를 Promiscuous mode로 바꿀 경우 2계층과 3계층에서 필터링 하지 않음. 
	- Promiscuous mode는 간단한 설정 사항이나 스니핑을 위한 드라이버 설치를 통해 바꿀 수 있다
### Capture Filter
- 캡쳐를 시작하기 전에 필터를 적용하는 방식
- 필요성1: 필터 없이 캡쳐를 시작할 경우 많은 양의 패킷으로 인해 원하는 패킷을 찾기 힘들다.
- 필요성2: 캡쳐를 시작한 이후 Display Filter를 적용할 수 있으나, Display Filter는 모든 패킷을 캡쳐 한 후, 필터링을 진행 하는 것이기 때문에 불필요한 패킷이 캡쳐되어서 파일의 크기가 커진다.
- 이때 Capture Filter를 적용하면, 불필요한 패킷은 사전에 필터링되어서 Wireshark 프로그램에 걸리는 부하를 효과적으로 줄일 수 있음
- 특정 호스트의 패킷 캡쳐 
	- host 10.10.50.170 
- 특정 두 호스트의 통신 패킷 캡쳐 
	- host 10.10.50.170 and host 10.10.50.15 
- 특정 포트 패킷 캡쳐 
	- port 80 
- 특정 두 포트 전부 패킷 캡쳐 
	- port 80 or port 1770 
- 특정 호스트의 특정 포트 패킷 캡쳐 
	- host 10.10.50.170 and port 80 
- 특정 패킷만 캡쳐 안함 
	- not arp
< 필터 사용법 > &&와 and 모두 사용 가능, \==와 eq 모두 사용 가능, || 와 or 모두 사용 가능
![[Pasted image 20250522115717.png|300]]
### Display Filter
- 캡쳐를 한 뒤 원하는 패킷만 화면에 출력한다. 
- Capture filter 와 문법이 다르다. 
- 원하는 패킷을 캡쳐한 뒤 세부 분석을 하기 위해 쓰인다

- 출발지나 목적지 IP 주소로 검색 
	- ip.addr == 192.168.0.1 
- 출발지 IP 주소로 검색 
	- ip.src == 192.168.0.1 
- 도착지 IP 주소로 검색 
	- ip.dst == 192.168.0.1 
- 포트로 검색 
	- tcp.port == 443 
- 출발지 포트로 검색 
	- tcp.srcport == 443 
- 도착지 포트로 검색 
	- tcp.dstport == 443
![[Pasted image 20250522115706.png|300]]
## FTP
파일 전송 프로토콜
TCP/IP 프로토콜을 가지고 서버와 클라이언트 사이의 파일 전송 을 하기 위한 프로토콜
FTP를 사용하지 말아야 하는 이유? 평문 파일 전송 프로 토콜이기 때문
## ARP