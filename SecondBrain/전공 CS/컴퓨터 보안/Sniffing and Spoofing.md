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