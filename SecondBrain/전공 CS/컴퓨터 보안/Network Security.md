## Dos and DDos attacks
■ 취약점 공격형
	● Boink, Bonk, TearDrop
	● Land
■ 자원 고갈형
	● Ping of death
	● SYN flooding
	● HTTP flooding
	● Smurf
	● Mail bomb
### TearDrop
▣ TCP의 오류 제어 로직의 불완전한 구현을 악용
	■ 특히 Windows와 linux의 오래된 버전의 fragmentation reassembly bug
▣ 공격 방식
	■ 클라이언트 측에서 의도적으로 패킷의 sequence number와 길이를 조작
	■ 패킷들이 부분적으로 겹치거나 빠진 패킷이 있어 reassemble 불가
![[Pasted image 20250520113039.png|200]]
▣ 대응 
	■ 수신되는 패킷의 frame alignment를 확인하여 filtering
### LAND(local area network denial) Attack
▣ 일종의 packet spoofing 공격
▣ source와 destination IP address를 모두 target host의 IP address로 설정하고 TCP SYN 패킷을 전송
▣ target host는 자신에게 지속적으로 응답을 하게 됨
▣ 대응
	■ source와 destination IP address 유효성 검증
### Ping of Death
▣ ping 명령어에 최대한 긴 패킷을 실어서 공격 대상에게 전송
	■ 정상적인 ping packet은 보통 56 바이트
	■ ICMP (Internet Control Messaging Protocol)헤더가 고려될 경우 64바이트, IPv4 헤더 포함해도 84 바이트
▣ IPv4 패킷 크기는 65,535 바이트까지 가능하나, 경유 네트워크 특성에 따라 전송 중에 수백~수천 개의 패킷으로 분할됨
	■ 분할된 패킷이 다시 합쳐져서 전송되는 일은 거의 없으므로, 공격 대상 시스템은 대량의 작은 패킷을 수신하게 되어 리소스 소모
	■ 일부 TCP/IP 시스템은 최대값보다 큰 패킷을 처리하도록 설계되지 않았으므로 해당 크기를 초과하는 패킷에 대해서는 크기 제한 초과에 의한 버퍼 오버플로우 발생 가능
▣ 대응
	■ 주로 방화벽(firewall)에서 ping이 사용하는 ICMP를 차단하여 해결
### SYN flooding
▣ 시스템에서 허용하는 동시 사용자 수 제한과 TCP 3-way handshake 특성을 악용
▣ 동작 과정
	■ TCP에서 서버가 클라이언트로부터 SYN을 받으면 SYN+ACK을 회신하고 ACK가 올때까지 일정 시간을 기다림 (SYN Received 상태 유지)
	■ 공격자는 가상의 클라이언트로 위조한 SYN 패킷을 여러 개 만들어 서버에 보냄으로써, 서버의 가용 동시 접속자 수를 모두 점유(모두 SYN Received 상태로 만듬)
▣ 대응
	■ SYN Received 상태의 대기 시간을 줄이는 등 다양한 해법 존재
	■ IETF RFC 4987, “TCP SYN Flooding Attacks and Common Mitigations”
### HTTP flooding
▣ 웹 서버에 대한 DoS/DDoS 공격
▣ 기본적인 공격
	■ GET flood
		● image와 같은 특정 static content를 지속적으로 요구(GET request)
	■ POST flood
		● 서버 내의 database 검색 등 특정 동작을 지속적으로 요구
	■ 대응
		● traffic profiling (특징적인 HTTP 요청 패턴을 검출하고 차단)
▣ 보다 진화된 공격
	■ slow HTTP POST (= RUDY (RU-Dead-Yet?))
		● 서버가 POST 데이터를 모두 수신하지 않았다고 판단하면 전송이 다 이루어질때 까지 연결을 유지하는 특성을 활용, POST 메소드로 대량의 데이터를 장시간에 걸쳐 분할 전송하여 연결을 장시간 유지함으로써 서버의 자원 잠식
		● 예를 들어 Content-Length를 100000byte로 하고 데이터는 일정한 간격으로 1byte씩 전송
	■ slow HTTP header (slowloris attack)
		● HTTP header 정보를 비정상적으로 조작하여 웹서버가 완전한 header 정보를 기다리도록 함
		● 예를 들어, HTTP에서 header의 끝을 개행문자 /r/n (CR LF)로 구분하므로, header 시작 후 개행 문자 없이 의미없는 문자열을 보내면 서버는 계속 연결을 유지
	■ 대응
		● 세션 임계치 설정, 세션 타임아웃 시간 제한
### Smurf attack
▣ Direct broadcast
	■ 기본적인 broadcast는 destination IP address 255.255.255.255로 패킷을 전송하며, 이는 라우터 경계 내에서만 동작함
	■ direct broadcast: 라우터를 넘어가서 broadcast를 해야 하는 경우 IP address 의 일부분만 broadcast 주소로 채움
		● 예: 165.246.13.255 → 165.246.13 으로 시작하는 subnetwork를 대상으로 broadcast
▣ Smurf attack
	■ Direct broadcast를 악용하여, source IP address를 특정 주소(피해자의 IP address)로 조작한 ICMP request 를 broadcast
	■ 피해자는 수많은 ICMP reply를 받아 과부하 상태가 됨
▣ 대응
	■ 각 host와 router들로 하여금 broadcast address 일 경우 무시하도록 설정
