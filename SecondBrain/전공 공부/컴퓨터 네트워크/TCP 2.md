# TCP Timers
![[Pasted image 20241030150552.png|500]]
### Persistence Timer
- rwnd가 0인 ACK을 받았을 때 켜지고, timer가 expire되면 작은 사이즈의 데이터(probe segmenr)를 보낸다.
- 교착상태를 해결하기 위하여 사용
- 영속타이머가 만료되면, probe 세그먼트 전송
### Keepalive Timer
- 클라이언트가 하드웨어적으로 죽으면 FIN을 보내지 못하고 죽어버림 이를 서버가 계속 가지고 있지 않기 위해 2시간 동안 가지고 있다가 세그먼트를 전송함.
- 오랜 기간 동안 idle 상태에 있는 것 방지
- 서버가 2시간 동안 클라이언트로부터 세그먼트를 전송
	- 받지 못하면, probe 세그먼트 전송

