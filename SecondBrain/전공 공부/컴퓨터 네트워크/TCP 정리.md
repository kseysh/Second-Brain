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
### UDP
TCP 와 다른 방식인 Boundary delivary를 이용한다.
메시지(application layer의 packet)에서 만들어진 boundary가 전달이 될 때 유지된다.
ex) 김사장님이 비서에게 사탕 4개를 주고, 이 사탕 4개는 항상 한 박스로 포장해서 보내라고 지시함

## TCP numbering
TCP는 보내는 각 