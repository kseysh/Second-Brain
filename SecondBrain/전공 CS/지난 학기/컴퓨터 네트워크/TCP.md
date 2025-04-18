# TCP (Transmission Control Protocol)

## Stream delivary
![[Pasted image 20240915230324.png|300]]
TCP는 Application Layer에서 만든 boundary를 상관하지 않고 보낸다
UDP는 이와 다르게 application layer에서 만들어진 message의 boundary가 전달이 될 때 유지되는 Boundary delivary를 이용한다.

![[Pasted image 20240915230428.png|300]]
여기서는 보내는 것만 보여줬지만, Sending Buffer에서 보낸 것을 Listening Buffer에서 받았다면, 받은 쪽에서는 어떤 요청을 보낸 것인지 확인하고 다시 자신의 Sending Buffer를 이용해 요청한 쪽의 Listening Buffer로 데이터를 보내준다.

Buffer는 세가지 영역을 가진다. sent, not sent, empty 
sent: tcp는 요청을 받는 쪽에서 데이터를 받지 못했을 때 다시 보내줘야 한다. 이를 위해 데이터를 보내도 이를 삭제하지 않고 카피본을 만들어둔다. (application layer에게 다시 데이터를 요청하지 않기 위해서) 이를 sent 영역이라고 한다.

이 버퍼는 프로세스(port num)마다 하나씩 만들어진다.
이 버퍼는 연결 요청 set up 된 후 에 생성되고, 연결 종료시에 사라진다.
## TCP numbering
TCP는 보내는 각 패킷마다 번호를 붙인다. 
번호는 랜덤으로 생성된 번호를 붙인다.

ex)
한 세그먼트가 1000 byte일 때, 5000byte를 보내기 위해서는 5개가 필요하다.
시작 번호는 랜덤으로 선택된다.
번호는 각 세그먼트의 시작번호로 설정한다.
sequence number: 패킷에 있는 첫 번째 byte 번호 
![[Pasted image 20240915231643.png|450]]

## TCP가 패킷을 받았을 때 응답하는 방법
![[Pasted image 20240923233232.png|300]]
### Selective ACK
\#101을 받았다면, ACK\#101 (101을 받았다)을 응답
\#201을 받았다면, ACK\#201 (201을 받았다)을 응답
- 직관적이고 보편적인 방식
### Cumulative ACK
\#101을 받았다면, ACK\#201 (201을 받고 싶다)을 응답 (이 전까지는 제대로 받았다는 것을 의미)
\#201을 받았다면, ACK\#301 (301을 받고 싶다)을 응답
- TCP가 사용하는 방식
## TCP Header
![[Pasted image 20240923233720.png|350]]
Window size: receiver의 window size, 자신이 받을 수 있는 빈 공간
### Acknowledgment number
Receiver가 다음 번에 받고 싶은 바이트 번호
### HLEN(header length)
TCP 헤더가 얼마만큼의 크기를 가지는지 알기 위해 사용 
(TCP 헤더는 기본 20byte지만 옵션을 써서 더 늘릴 수 있다. 옵션을 썼는지 확인하기 위해서는 HLEN을 확인한다.) 
#### 특징
비트 수를 아끼기 위해 비트를 4로 나눈 값을 사용한다. 
따라서 4씩 증가한다. 20, 24, 28... 60
ex) 헤더가 60byte라면 4로 나눠 1111을 HLEN으로 표기한다.
### Control field
![[Pasted image 20240923235851.png|350]]
ACK: ACK 번호(Acknowledgment number)를 보내니까 체크해야함을 알려주는 bit (0 or 1)
SYN: 연결 요청 패킷
FIN: 종료 요청 패킷
나머지는 잘 몰라도 됨
### Reserved
나중에 사용할 수도 있어서 추가적으로 잡아놓은 field
### Checksum
패킷들이 전송되다가 물리적으로 어떤 비트가 전송중에 오류가 발생하는 것을 체크하기 위해서 필요한 것
오류가 발생했다고 판단하면 패킷 전체를 버린다.
TCP에서는 Checksum이 필수, UDP에서는 Optional이다.
# TCP three-way handshake Connection Set Up
TCP는 연결하기 전에 Set up 과정을 거친다.
set up 과정을 거치고, set up이 다 되고 나서야 데이터를 보낸다.
![[Pasted image 20240924005019.png|450]]
rwnd: receiver의 window size, 자신이 받을 수 있는 빈 공간이 5000이다. (byte 단위이다.)

서버는 먼저 실행되어서 상대방의 연결 요청을 하는 것을 기다려야 한다.
TCP에서는 서버가 먼저 준비되어 있어야 한다.
### cli -> serv `sin` 
연결 해도 돼?
sequence Number로 8000 전달 (random number 생성)
### serv -> cli `ack`
연결 허락
ack로 8001번을 전달하여 8000번 까지 잘 받았고, 8001번부터 달라고 요청
### serv -> cli `sin`
연결 허락 패킷이 잘 갔는지 확인
cli에게 패킷이 잘 갔는지 확인해야 하므로 여기에도 번호를 부여한다 (seq: 15000)
seq는 random number(15000)로 시작해서 보낸다.
### cli -> serv `ack`
패킷이 잘 왔다는 응답
control field에 S가 없어 sin은 읽지 않는다.
### 설명
SYN packet은 데이터를 가져가지 않고 헤더만 전송된다. sequence number 하나만 사용한다.
SYN + ACK도 데이터를 가져가지 않고 헤더만 전송된다. sequence number 하나만 사용한다.
ACK packet은 데이터를 가져가지 않고 sequence number도 사용하지 않는다.

TCP set up이 끝나면 데이터의 sequence number는 8001번부터 쓴다.
## three-way handshake 이후 데이터 전송 
![[Pasted image 20240924011011.png|450]]
SYN이 가면 ACK이 오는데, request가 갈 때 어차피 헤더 ACK도 보내야 하므로 데이터를 같이 보낸다.
ACK만 갈 때 seq 번호는 의미가 없다.

# TCP three-way handshake Connection termination
![[Pasted image 20240924011944.png|450]]
FIN: SYN처럼 연결 종료 패킷을 보내는 것
ACK: 종료 요청 허락
FIN: serv도 종료 요청을 하는 것
ACK: 종료 요청 허락
### 설명
FIN만 갈 때는 데이터는 없고 sequence number를 갖는다.
FIN + ACK이 갈 때는 데이터는 없고 sequence number를 갖는다.

## Half-Close
상대방이 종료 요청을 해도 나는 종료하지 않을 수 있다.
![[Pasted image 20240924013837.png|450]]
종료 요청은 클라이언트가 먼저하는 것이 일반적이다.
하지만 만약 서버가 더 보낼 내용이 있다면 데이터를 다 보낸 이후에 FIN을 보낸다.
위 그림에서 현재 client는 FIN을 보낸 상태라 더 데이터를 못 보내지만 그 대신 서버는 보낼 데이터를 다 보내고 FIN을 보낸다. 클라이언트는 다른 요청은 보내지 못하지만 ACK로 응답은 해줄 수 있다.
### 종료 시에 보통 4-way-handshake를 하는 이유
FIN-ACK을 같이 보내면 서버가 남은 데이터를 전송하지 못하므로 데이터 손실을 방지하고 송수신 방향을 독립적으로 종료하기 위해 4-way-handshake를 진행한다.

## State transition diagram
![[Pasted image 20241002150905.png|500]]
받은 것 / 보내는 것의 순서대로 되어 있다
중요한 그림이니 이해하기
### client
SYN과 SYN+ACK 사이의 상태를 이름을 지은 것 = SIN-SENT
SYN/ACK이 되면 ACK을 보내고 ESTABLISHED 상태(데이터를 주고 받을 수 있는 상태)가 된다

FIN을 보낸 상태 FIN-WAIT-1
ACK을 받은 상태 FIN-WAIT-2
FIN을 받은 상태 TIME-WAIT (바로 끝나지 않고 Maximum Segment Lifetime(msl) 만큼 기다렸다가 CLOSE로 변한다)
ACK을 보낸 상태 CLOSED
### server
SYN을 받기 전 상태 = LISTEN
SYN을 받은 상태 SYN-SENT
ACK을 받은 상태 ESTABLISH

FIN을 받고 ACK을 보낸 상태: CLOSE-WAIT
FIN을 보낸 상태 LAST-ACK
ACK을 받은 상태: CLOSED (버퍼 및 소켓 삭제)
![[Pasted image 20240925152032.png|500]]
서버 소켓: syn을 받으면 syn/ack을 보내주는 역할을 함 (일반 소켓은 못 보냄)
listen: syn/ack을 보내주도록 서버 소켓으로 만드는 함수
accept: ack을 받는 함수

## Time wait이 필요한 이유
![[Pasted image 20240925154840.png|300]]
close를 하고 다시 syn syn/ack ack을 통해 바로 연결요청을 하는 상황이다.
### 1
서버는 이전 연결과 이후 연결을 포트 번호로 구분한다.
이후 연결이 IP 주소와 port번호를 똑같이 다시 오게 되면 새로운 것이 온건지 이전 것이 온건지 구분하기가 어려워진다.
따라서 3456 포트를 반납하지 않고 Time wait을 걸어 못쓰게 만든다. (3456포트에서 요청이 들어오면 무시한다.)
어차피 패킷은 1-2분 후에 자동 소멸 하므로 Time wait만큼 기다리면 다시 요청이 들어오지 않게 된다.
### 2
FIN/ACK을 보내고 ACK을 못 받았을 때, ACK을 다시 요청하면 ACK을 보내야하므로 대기 시켜놓는다.
### 3
fin을 먼저 보내는 애가 Time-wait이 된다.
만약 서버가 ^C로 죽어버리면 time-wait때문에 port를 못 쓸 수도 있는데 이는 time-wait을 하지 않는 옵션을 사용해서 해결할 수 있다.
## WINDOWS IN TCP
SYN을 보내고, 기다렸다가 ACK을 받고 다시 SYN을 보내기(STOP&WAIT 방식)에는 시간이 너무 많이 든다.
따라서 한 번에 SYN을 n만큼 보내기 위해서, 받는 쪽에서 받을 수 있는 패킷의 수를 고려한다. 

처음 받는 쪽에서 패킷의 빈공간은 Window Size를 통해 정한다.

![[Pasted image 20241002154019.png]]
- 최초에 아무 것도 안 보냈을 때 rwnd는 syn/ack시에 알 수 있다.
- 상대방이 rwnd를 100이라고 알려주면, rwnd에서 내가 보낸 것을 빼서 그 만큼 더 보낼 수 있겠다고 인지한다.
- 현재 ACK \#201을 받은 상황이고 따라서, 200 부터는 sender buffer에서 관리하지 않아도 된다.
	- 슬라이딩 윈도우처럼 관리한다.
- buffer에서 Sent 영역: 보냈지만, 아직 받았다고 ACK을 받지 않는 것
## Flow control
![[Pasted image 20241002155355.png]]
window size는 rwnd와 cwnd의 minimum으로 관리한다.
두 값은 독립적이고 rwnd는 receiver의 입장만 보고, sender는 network 상황을 보고 cwnd를 관리한다.

![[Pasted image 20241002155812.png]]
보내는 양이 어떻게 조절되는지를 보여주는 그림
application에서 tcp함수로 write함수를 통해 데이터를 작성한다.
read 함수는 block함수이지만, write 함수도 block 함수이다. 버퍼에 빈 공간이 꽉찼고, write할 데이터가 더 크면 block된다.
버퍼에 write할 데이터가 확보되어야만 write할 수 있다.
전송 속도는 application에서 소비하는 속도에 맞도록 선택된다.
3번이 느려지면, 느려지고, 3번이 빨라지면 빨라진다.
### ex)
![[Pasted image 20241002160450.png]]
그림의 client에서 분홍색은 client의 application에서 server에게 데이터를 전송하라고 갖다놓은 데이터, 만약 applicaiton이 800만큼 갖다놓으면 800을 전송했을 것임.
client의 왼쪽 window 벽: server의 ackNo 값
client의 오른쪽 window 벽: server의 ackNo + rwnd
server의 왼쪽 window 벽: client의 seqNo + Data 값
server의 오른쪽 window 벽: 기존 window 오른쪽 벽에서 application이 consume한 data 값을 더해줌
client->server 단방향에서 설명하는 것.
rwnd: 현재 내가 보낼 수 있는 최대한의 바이트
매 순간마다 얼만큼 데이터를 보낼지를 결정함.

## Silly Window Syndrome
보내는 데이터가 헤더에 비해서 너무 작아서 발생하는 비효율
### 송신 측에서 발생하는 지연 해결책
#### Nagle 알고리즘
보낼 데이터가 MSS(Max Segment Size)로 정의된 크기만큼 쌓이면, 상대편에게 무조건 보냄
보낼 데이터가 MSS보다 작을 경우, 이전에 보낸 데이터에 대한 ACK이 오기를 기다림.
ACK이 도달하면 보낼 데이터가 MSS보다 작더라도 상대에게 보냄

처음 데이터는 그냥 보냄, 두 번째 데이터부터는 MSS만큼 다 찼을 때 보내거나, 이전에 보낸 데이터에 대한 ACK이 올 때 데이터를 보냄
![[Pasted image 20241010171114.png|400]]
### 수신 측에서 발생하는 지연 해결책
#### Clark 해결방법
MSS만큼 비거나 버퍼의 반이 비면 제대로 된 rwnd를 보내주고, 그렇지 않으면 rwnd를 0으로 보낸다.
#### 확인 응답의 지연
버퍼에 일정 크기 이상의 빈공간이 생겼을 때 ACK을 보내고, 아니면 보내지 않는다. 
#### 두 방식의 차이
Clark 해결방법: 상대방이 특정 조건에서 rwnd를 0으로 보내 바로 정지할 수 있음
확인 응답의 지연: 특정 조건에서 ACK을 보내지 않아 조금은 천천히 정지함
### SYN Flooding
![[Pasted image 20241010195743.png|450]]
Client1은 서버에게 SYN을 보내고 서버는 Client에게 SYN/ACK을 보내고 연결 요청 대기 큐에 대기시켜놓는다. (서버 소켓의 역할)
Client의 ACK이 서버에게 오면 accept 함수가 호출되고 연결 요청을 수락한다.

만약 요청이 많거나, 악의적으로 ACK을 받을 수 없는 SYN만 보내면 연결 요청 대기 큐가 가득 차서 유저가 연결을 하지 못하게 만드는 것을 SYN Flooding이라고 한다. (D-Dos)
## TCP Normal operation
![[Pasted image 20241010201504.png]]
### Rule 1,2,3 
ACK을 더 효율적으로 보내기 위해 할 수 있는 방법
#### Rule 1
데이터를 받고 ACK을 보낼 때 Sending Buffer를 찾아보고, 보낼 수 있는 데이터를 같이 보낸다.
#### Rule 2
ACK을 보낼 때 데이터가 없다면 50ms를 기다리고 데이터가 없으면 ACK을 보낸다.
#### Rule 3
ACK을 보낼 때 50ms를 기다리다가 패킷이 하나 더 오면 바로 ACK을 보낸다.
데이터가 없는 상황에 패킷 두개가 오면 적어도 두개당 패킷 하나는 보내자(ACK이 없어지면 데이터가 잘 받았는지 확인할 방법이 없기 때문 => TCP의 안전을 위해)
![[Pasted image 20241011164502.png]]
### Rule 4,5
ACK의 개수를 줄이다가 응급 상황이 발생했을 경우 바로 ACK을 보내자
#### Rule 4
패킷이 사라진 것을 알았을 때, 사라진 것에 대한 ACK을 보내자 
Q. 패킷이 사라진 것인지, 단디 701-800이 801-900 보다 늦게 온 것인지 어떻게 detect하는지?
A. 중복된 ACK이 계속 오면 패킷이 늦게 온 것이 아닌 사라졌을 확률이 높다고 간주하고, 재전송하는 것.
#### Rule 5
못 받은 패킷이 도착하면(Recent된 ACK이 오면), 바로 응답을 보내자
Q. 못 받은 패킷인지에 대한 확인은 어떻게 하는지? 중간에 채워진 데이터면 ACK을 보내는건가? Fast retransmit이 된 패킷에 대해 ACK을 보내는 것인가?

![[Pasted image 20241011172027.png]]
#### Rule 6
중복된 패킷을 받는 경우도 있다. 그럴 때는 ACK을 기다리지 않고 응답을 바로 보낸다.
## Fast retransmission
![[Pasted image 20241011170511.png]]
원래는 Timeout까지 가서 패킷 전송이 잘못되었음을 확인하고 Resent를 하지만, 중복된 ACK이 계속 오면 패킷이 늦게 간 것이 아닌, 패킷 전송이 잘못되었을 확률이 높다고 간주하고 다시 Resent를 한다.
따라서 이 개수를 정한다: 3개의 중복된 ACK이 오면 패킷 전송이 잘못되었다고 가정하고 Fast retransmit(빨리 재전송)을 한다.
## Lost acknowledgment
![[Pasted image 20241011171759.png]]
Sender는 Ack: 701을 받았지만, 이후에 받은 Ack으로 Receiver가 900까지는 잘 받은 것을 확인할 수 있다.

#### 이로서 알 수 있는 cumulative ACK의 장점
2개당 1개의 ACK을 보내서 ACK의 개수를 줄일 수 있다.
Fast retransmit같은 것을 쉽게 구현할 수 있다. (쉽게 packet lost를 detect가능)
Receiver의 ACK이 lost되어도 Resent하지 않아도 되는 Case가 있다.
## ack 유실로 인해 DeadLock이 발생할 수 있다.
Receiver는 buffer에 충분한 공간이 없어 rwnd를 0으로 보낼 수 있다.
이 때, buffer에 충분한 공간이 생기면 다시 Receiver는 ACK을 이용해 rwnd값을 보내는데, 그 ACK이 유실되면 DeadLock이 발생할 수 있다. 
하지만, ACK에 대한 ACK은 없기 때문에 Receiver는 Sender가 ACK을 잘 받았는지 확인할 수 없다.
현재 상황 (Dead Lock)
Sender: Receiver에게 rwnd = 0을 받아 대기 중
Receiver: Sender에게 rwnd = k라는 ACK을 보내고 Sender가 데이터를 보내주기를 대기 중.
### 해결 방법
#### Persistence Timer
교착 상태를 해결하기 위하여 사용
Persistence Timer가 만료되면 probe 세그먼트 전송
1. Sender는 rwnd=0을 받으면 Persistence Timer를 켠다.
2. 일정 시간이 지나면 작은 사이즈의 데이터를 보내본다. (헤더가 낭비되긴 하지만 특별한 케이스이므로) => probe segment라 한다.

## Congestion Control
![[Pasted image 20241011173930.png|400]]
TCP connection 1,2 모두 100Mbps로 데이터를 보내고, 라우터도 최대 100Mbps로 보내는 것이 가능하다면 bottle neck이 발생하게 된다.
packet switching은 circuit switching에 반해 중앙 경로 설정이 아니기 때문에, 모니터링이 불가능하고 성능에 관한 보장도 불가능하다.
ex) packet switching: 자동차 길 circuit switching: ktx
따라서 packet switching 방식은 혼잡도를 측정할 방법이 마땅하지 않다.

![[Pasted image 20241011175158.png|250]]
Capacity를 넘게 되면 지수적으로 delay되는 시간이 길어진다.

![[Pasted image 20241011175223.png|250]]
capacity가 최대 10Mbps라 할 때, 50Mbps를 보낼 때 5초가 걸렸다면 Throughput은 10Mbps라고 할 수 있다.
하지만 점점 혼잡도가 증가해 10초가 걸렸다면 Throughput이 5Mbps로 줄어들게 된다.

## Addtive increase
![[Pasted image 20241016152812.png|500]]
cwnd: congestion window size (구현은 byte 단위, 이해는 packet 단위로 하자)
이 그림에서는 이해를 위해 한 패킷에 한 ACK으로 생각하자.
여기서 혼잡을 패킷이 없어진 것으로 생각하자. (하드웨어 이슈일수도 있지만 혼잡때문에 패킷 loss가 생기는 것이 확률이 99%이상이다.)
처음에는 4개 패킷, 4개의 ACK, 5개 패킷, 5개의 ACK이 보내지면서 혼잡을 확인한다. (조금씩 늘려보면서 혼잡을 확인한다.)
이는 RTT마다 증가한다.
혼잡이 발생할 때까지 증가한다.
## exponential increase
![[Pasted image 20241016153339.png|500]]
set up 이후에 1,2,4,8,16,32 이렇게 두 배씩 데이터를 증가하면서 보내본다.
이게 아니라면 receiver의 rwnd만큼 보내기 때문에 

언제까지 증가하냐면, 정해놓은 threshold(system에서 default로 설정한 값)만큼이거나, 혼잡으로 인한 packet loss가 발생할 때.

![[Pasted image 20241016153928.png]]
 Time-out이 발생하면 threshhold를 반으로 줄이고 cwnd를 1로 한다.
3 duplicate ACKs는 cwnd를 반으로 줄인다.

![[Pasted image 20241016153934.png]]
최초의 cwnd는 1로 설정한다.
여기서 system에서 정한 Threshold는 16
현재 패킷 단위로 설명.
Threshold를 만나면 slow start는 stop하고 Additive increase를 한다.
혼잡이 발생했다고 판단되면(Fast transmission이 진행되면) cwnd를 반으로 줄인다.

혼잡은 Time-out으로도 감지될 수 있다. (혼잡이 심각하면 duplicate ack도 안 옴 time-out밖에 답이 없음) => 심각한 혼잡으로 판단
그래서 Time-out으로 감지된 혼잡은 1에서 다시 시작한다.
Timeout이 발생해서 Time-out이 발생한 20에서 Threshold를 반으로 줄인다.
Threshold에서는 항상 stop하고, addtive increase를 한다.

## TCP가 공평한 원리
![[Pasted image 20241019171431.png|500]]\
전제 조건: 
- 두 개의 RTT가 똑같다.
- additive increase만 적용하고 있다고 가정한다.

additive increase이므로 y=x의 기울기와 같이 움직이지만, 3 dupplicate ack이 발생하여 둘 다 cwnd가 절반으로 감소하고 이 것을 반복하다 보면 점점 두 throughput이 동일하게 된다.

![[Pasted image 20241019174035.png|300]]
throughput은 RTT와 반비례하다.
RTT가 다르면 수렴하는 기울기가 변한다. (이 상황에서는 connection1이 connection2보다 RTT가 2배 작음)
또한, connection1이 process를 두개 사용하게 되면(socket을 두 개 사용하게 되면) TCP는 세 개의 소켓을 공평하게 throughput을 나누기 때문에 connection끼리의 throughput의 비율이 달라질 수 있다.
따라서 RTT와 connection의 개수에 따라 fair하지 않은 상황이 있을 수 있다.

# TCP의 중요한 개념 4가지
Set up 과정: syn syn/ack ack
Reliablility: seq num과 ack num을 이용해 packet loss를 detect함
flow control: 상대방이 얼마나 받을 수 있는지 확인하며 보냄
congestion control: 혼잡이 일어나면 조금씩 보내는 양을 줄이면서 적절한 보내는 양을 찾음
### UDP는?
UDP는 TCP의 중요한 개념 4가지를 모두 하지 않는다.
`TCP는 전화, UDP는 엽서` => 엽서는 상대방이 받지 않아도 모름
## *UDP는 빠르다?*
패킷 양이 작으면 UDP가 setup과정을 거치지 않으므로 빠를 수 있다.
하지만, 패킷 양이 크고, 네트워크가 받쳐준다면 TCP는 congestion control에서 cwnd 값을 점차 늘리면서 보내는 양이 일정한 UDP에 비해 보내는 양이 더 커지면서 TCP가 더 빠르게 동작할 수도 있다.