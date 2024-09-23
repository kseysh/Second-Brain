# TCP (Transmission Control Protocol)
client는 임시 port 번호를 이용해 

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
![[Pasted image 20240915231643.png]]

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
