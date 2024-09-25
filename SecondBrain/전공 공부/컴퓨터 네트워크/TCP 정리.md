# TCP (Transmission Control Protocol)

## Stream delivary
![[Pasted image 20240915230324.png|300]]

TCP에서 사용되는 방식

ex) 김사장님은 비서에게 사탕 4개, 과자 3개, 음료 5개를 줌, 비서는 이것들을 한 바구니에 차곡차곡 담는다
비서가 확인했을 때 택배 사이즈가 6개가 들어갈 수 있는 크기라면 비서는 이를 6개씩 묶어서 보낸다 (메시지에서 만든 boundary를 상관하지 않고 보낸다.)
김사장님은 비서에게 3개를 보냈지만, 비서는 택배 회사에게 두 개만을 보낸다.
비서는 낱개만을 계산해서 쭉 줄 세워놓고, 필요한만큼 끊어서 택배 회사로 보낸다.
이는 바이트 단위로 보내는 것처럼 보인다. 따라서 Stream delivary라고 불린다.
![[Pasted image 20240915230428.png|300]]
여기서는 보내는 것만 보여줬지만, Sending Buffer에서 보낸 것을 Listening Buffer에서 받았다면, 받은 쪽에서는 어떤 요청을 보낸 것인지 확인하고 다시 자신의 Sending Buffer를 이용해 요청한 쪽의 Listening Buffer로 데이터를 보내준다.

Buffer는 세가지 영역을 가진다. sent, not sent, empty 
sent: tcp는 요청을 받는 쪽에서 데이터를 받지 못했을 때 다시 보내줘야 한다. 이를 위해 데이터를 보내도 이를 삭제하지 않고 카피본을 만들어둔다. (application layer에게 다시 데이터를 요청하지 않기 위해서) 이를 sent 영역이라고 한다.

이 버퍼는 프로세스(port num)마다 하나씩 만들어진다.
이 버퍼는 연결 요청 set up 된 후 에 생성되고, 연결 종료시에 사라진다.
### UDP
TCP 와 다른 방식인 Boundary delivary를 이용한다.
메시지(application layer의 packet)에서 만들어진 boundary가 전달이 될 때 유지된다.
ex) 김사장님이 비서에게 사탕 4개를 주고, 이 사탕 4개는 항상 한 박스로 포장해서 보내라고 지시함

## TCP numbering
TCP는 보내는 각 패킷마다 번호를 붙인다. 
번호는 랜덤으로 생성된 번호를 붙인다.

ex)
한 세그먼트가 1000 byte일 때, 5000byte를 보내기 위해서는 5개가 필요하다.
시작 번호는 랜덤으로 선택하는데, 이는 10001으로 설정하였다.
택배상자의 번호는 각 세그먼트의 시작번호로 설정한다.
sequence number: 패킷에 있는 첫 번째 byte 번호 
![[Pasted image 20240915231643.png|450]]

sequence number는 Data의 첫 번째 byte 번호이다.
![[Pasted image 20240915232219.png|350]]

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
Data: payload라고도 부른다.
Sequence number: 
### Acknowledgment number
Receiver가 다음 번에 받고 싶은 바이트 번호
### HLEN(header length)
헤더가 얼마만큼의 크기를 가지는지 알기 위해 사용 
(헤더는 기본 20byte지만 옵션을 써서 더 늘릴 수 있다. 옵션을 썼는지 확인하기 위해서는 HLEN을 확인한다.) 
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
![[Pasted image 20240924001817.png|400]]
IP헤더에도 있는 값이다.
Header에서 16bit씩 가져와서 다 더하고, 보수 값(0은 1로 1은 0으로)을 취한게 checksum이다.
Receiver도 확인할 때, checksum을 빼고 더한 값이 똑같으면 전송 중에 error가 없이 들어왔다고 판단한다.
오류가 발생했다고 판단하면 패킷 전체를 버린다.
TCP에서는 Checksum이 필수, UDP에서는 Optional이다.
### Pseudoheader
![[Pasted image 20240924001446.png|300]]
TCP는 헤더에 Pseudoheader라는 것을 붙인다.
Pseudoheader에는 보내는 쪽 IP주소와 받는 쪽 IP주소를 붙인다. (이미 IP헤더에 있는 값이지만, 더 확실하게 체크하기 위해서 Pseudoheader를 붙여서 checksum을 구하고, 보낼 때는 Pseudoheader가 아닌 Header만 보낸다.)
또한 protocol과 TCP 전체 길이를 보낸다.
## 캡슐화
![[Pasted image 20240924004059.png|300]]
# TCP three-way handshake Connection Set Up
TCP는 연결하기 전에 Set up 과정을 거친다.
set up 과정을 거치고, set up이 다 되고 나서야 데이터를 보낸다.
![[Pasted image 20240924005019.png|450]]
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

 TCP는 모든 개념을 한 그림에 담기 어려워서 그림이 담는 key point를 잘 알아야 한다. (그림 볼 때 제목도 본다.)
3 way도 4 way도 가능하다. 상황에 따라 달라진다.
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

## socket 통신 함수
![[Pasted image 20240924013348.png|350]]
### server
socket: 소켓 만드는 함수
bind: IP 및 port 바인딩
listen: 소켓을 전화받는 상태로 바꿈 (일반 소켓에서 서버 소켓으로 바꿈)
accept: 상대방이 연결요청하는 것을 기다림
연결이 다 되고 read write를 이용해 데이터를 주고 받음
close: 연결 종료 함수
### client
socket: 소켓을 만든다
connect: server로 연결요청을 한다.
연결 요청이 다 되면 read 혹은 write 함수를 써서 데이터를 주고 받음
close: 연결 종료 함수

## Half-Close
상대방이 종료 요청을 해도 나는 종료하지 않을 수 있다.\
![[Pasted image 20240924013837.png|450]]
종료 요청은 클라이언트가 먼저하는 것이 일반적이다.
하지만 만약 서버가 더 보낼 내용이 있다면 데이터를 다 보낸 이후에 FIN을 보낸다.
위 그림에서 현재 client는 FIN을 보낸 상태라 더 데이터를 못 보내지만 그 대신 서버는 보낼 데이터를 다 보내고 FIN을 보낸다. 클라이언트는 다른 요청은 보내지 못하지만 ACK로 응답은 해줄 수 있다.

## State transition diagram
![[Pasted image 20240925151800.png|400]]
### client
SYN과 SYN+ACK 사이의 상태를 이름을 지은 것 = SIN-SENT
SYN/ACK이 되면 ACK을 보내고 ESTABLISHED 상태(데이터를 주고 받을 수 있는 상태)가 된다

FIN을 보낸 상태 FIN-WAIT-1
ACK을 받은 상태 FIN-WAIT-2
FIN을 받은 상태 TIME-WATE (바로 끝나지 않고 Maximum Segment Lifetime(msl) 만큼 기다렸다가 CLOSE로 변한다)
ACK을 보낸 상태 CLOSED
### server
SYN을 받기 전 상태 = LISTEN
SYN을 받은 상태 SYN-SENT
ACK을 받은 상태 ESTABLISH

FIN을 받고 ACK을 보낸 상태: CLOSE-WAIT
FIN을 보낸 상태 LAST-ACK
ACK을 받은 상태: CLOSED (버퍼 및 소켓 삭제)
![[Pasted image 20240925152032.png|400]]
서버 소켓: syn을 받으면 syn/ack을 보내주는 역할을 함 (일반 소켓은 못 보냄)
listen: syn/ack을 보내주도록 서버 소켓으로 만드는 함수
accept: ack을 받는 함수

## 상대방이 FIN을 보냈을 때 서버 소켓이 알아듣는 방법
![[Pasted image 20240925153538.png|400]]
Inform and send data in the queue plus EOF를 설명하기 위한 그림

```cpp
for(i=0; i<5; i++)
{
	clnt_sock=accept(serv_sock, (struct sockaddr*)&clnt_adr,&clnt_adr_sz);
	if(clnt_sock==-1)
		error_handling("accept() error");
	else
		printf("Connected client %d \n", i+1);
	while((str_len=read(clnt_sock, message, BUF_SIZE))!=0)
		write(clnt_sock, message, str_len)
		close(clnt_sock); // return 값이 0이 아닐 때까지 계속 read/write 한다.
} // 들어온 값을 상대에게 다시 돌려주는 함수이다.
```
새로운 소켓을 만들어서 read와 write를 한다. serv_sock은 연결을 받는 역할만 한다.
read 함수: buffer에 있는 값을 한 번에 읽는다.
상대방이 fin을 보내면 read의 return 값이 0이된다. (이외에는 0보다 크다)

![[Pasted image 20240925154752.png|400]]
서버소켓은 client1을 위한 클라이언트 소켓 하나를 만들어준다. (114에 전화하면 담당 전화상담사를 한 명 배정해준다)
서버소켓은 client2를 위한 클라이언트 소켓 하나를 만들어준다.
서버소켓은 client3를 위한 클라이언트 소켓 하나를 만들어준다.
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
