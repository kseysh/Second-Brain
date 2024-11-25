## IP Diagram
![[Pasted image 20241125162536.png|400]]
### Service type
![[Pasted image 20241125162553.png|400]]
![[Pasted image 20241125162609.png|400]]
서비스 타입은 잘 쓰이지 않음(TCP 헤더의 Argent pointer처럼)
### Total length
![[Pasted image 20241125163428.png]]
 헤더와 데이터를 포함한 Datagram의 전체 길이
 만약 1byte의 데이터를 보낸다면, TCP 기본 헤더 20 IP 기본 헤더 20 총 41byte를 보내게 된다.
 그 후 frame도 헤더와 트레일러를 붙이게 되는데 
 처음 frame을 디자인할 때 frame의 payload가 최소 46byte가 되도록 디자인 하여 만약 41byte가 minimum이므로 이를 채우기 위해 Padding값이 들어가게 된다.
 상대방은 이 값을 받아 Padding만큼은 버려야하는데, 따라서 어느정도 길이까지가 표현해야 하는 값이다라는 것을 알려주기 우