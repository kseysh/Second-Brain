IP와 port number를 합친 개념으로 인터넷 상에 존재하는 각 port를 유니크하게 식별하기 위한 주소
각 socket은 인터넷 상에서 유니크하다.

socket은 각 [[connection]]을 유니크하게 식별할 수 있어야 한다.
=> 한 쌍의 socket은 connection을 유니크하게 식별한다.

하나의 socket은 동시에 여러 connection들에서 사용될 수 있다.
![[Pasted image 20240916023530.png]]
### UDP는?
connectionless: 연결을 맺지 않고 바로 데이터를 주고 받는다.]
unreliable: internet protocol을 거의 그대로 사용 (tcp가 아니므로)

