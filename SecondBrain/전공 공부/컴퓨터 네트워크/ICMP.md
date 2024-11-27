# Internet Control Message Protocol Version 4
## Position of ICMP in the network layer
![[Pasted image 20241127154718.png|500]]
![[Pasted image 20241127154832.png]]
type 번호 안 외워도 됨
## ICMP encapsulation
![[Pasted image 20241127154747.png|400]]
## General format of ICMP messages
![[Pasted image 20241127154915.png|400]]
code: 한 타입 안에 여러가지 경우의 수를 나타낼 때 사용



## Error-reporting messages
![[Pasted image 20241127155151.png]]





p4
icmp message에는 icmp header가 있고 data로서 ip header와 tcp 8yte가 있다.
p10
4페이지에 있는 것을 자세히 그린 그림이 10p 그림
패킷을 받아 ICMP packet을 만든다.
tcp 앞 헤더 8byte(source/destination port num과 seq번호)를 가져옴 
무슨 에러가 생겼는지는 ICMP를 보고, 어디서 에러가 생겼는지는 첫번째 IP header, 어디로 가려했는지는 뒤 IP header를 본다.
8byte는 어떤 프로그램에서 오류가 발생했는지 알려준다(TCP 앞 4byte로 port번호를 볼 수 있으므로)(TCP앞 4-8byte에서 몇번째 seq에서 오류가 발생했는지 알려준다.)

p11
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
p22

