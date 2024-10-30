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

TCP는 RTT를 측정해야 한다.
## RTO
### Smoothed RTT(RTT의 평균) - EWMA(exponential wait moving average) 방식
처음 => 아무 값도 설정 안됨
첫 번째 측정 이후 => (SYN을 보내고 SYN/ACK을 받았을 때의 시간) 
	`RTTs= RTTm`
그 이후의 측정 후 => 
	`RTTs = (1-α)RTTs + α RTTm`
	`RTTs`: 평균
	`RTTm`: measurement의 약자 (한 번 잘못 측정된 값이 영향을 너무 크게 줄까봐 1/8만 사용한다.)
α = 1/8
기존 7/8 x 예전 평균 + 1/8 x 현재 평균 

### RTT Deviation

처음 => 초기값
그 이후의 측정 후 =>
	`RTO= RTTs +4*RTTd`
	`RTTs`: 평균
	`RTTd`: 편차 (Timeout값을 여유있게 잡기 위해서 4를 곱해준다.)

RTT에 따라 Timeout값이 변화해야 한다
Timeout값을 너무 크게 잡으면 안된다.

## 실제 RTT
![[Pasted image 20241030151947.png|400]]