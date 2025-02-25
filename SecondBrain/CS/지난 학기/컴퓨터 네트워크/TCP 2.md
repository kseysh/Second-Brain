# TCP Timers
![[Pasted image 20241030150552.png|500]]
### Persistence Timer
- rwnd가 0인 ACK을 받았을 때 켜지고, timer가 expire되면 작은 사이즈의 데이터(probe segment)를 보낸다.
- 교착상태를 해결하기 위하여 사용
- 영속타이머가 만료되면, probe 세그먼트 전송
### Keepalive Timer
- 클라이언트가 하드웨어적으로 죽으면 FIN을 보내지 못하고 죽어버림 이를 서버가 계속 가지고 있지 않기 위해 2시간 동안 가지고 있다가 세그먼트를 전송함.
- 오랜 기간 동안 idle 상태에 있는 것 방지
- 서버가 2시간 동안 클라이언트로부터 세그먼트를 전송
	- 받지 못하면, probe 세그먼트 전송
## RTO 계산 방식
### `RTTs`(Smoothed RTT, RTT 평균) - EWMA(exponential wait moving average) 방식
처음 => 아무 값도 설정 안됨
첫 번째 측정 이후 => (SYN을 보내고 SYN/ACK을 받았을 때의 시간)
	`RTTs= RTTm`
그 이후의 측정 후 =>
	`RTTs = (1-α)RTTs + α RTTm`
	`RTTs`: 평균
	`RTTm`: measurement의 약자 (한 번 잘못 측정된 값이 영향을 너무 크게 줄까봐 1/8만 사용한다.)
α = 1/8
	`RTTs = (7RTTs + RTTm)/8`
기존 7/8 x 예전 평균 + 1/8 x 현재 평균 
RTTm => 가장 최근 측정된 RTT
### `RTTd`(RTT Deviation, RTT 편차) (EWMA 방식)
처음 => 아무 값도 설정 안됨
첫 번째 측정 이후 => 
	`RTTd= RTTm/2`
그 이후의 측정 후 => (편차는 절대값으로 구한다. 지금 측정한 값이 평균보다 얼마나 벌어져있는지)
	`RTTd = (1-ß)RTTd +ß |RTTs-RTTm|`
ß= 1/4
	`RTTd = (3RTTd + |RTTs-RTTm|)/4`
### RTO
처음 => 초기값
그 이후의 측정 후 =>
	`RTO= RTTs +4*RTTd`
	`RTTs`: 평균
	`RTTd`: 편차 (Timeout값을 여유있게 잡기 위해서 4를 곱해준다.)

RTT에 따라 Timeout값이 변화해야 한다
Timeout값을 너무 크게 잡으면 안된다.

## RTO 계산 방식 정리
처음, 첫 번째 측정, 그 이후의 순서
### RTTs
- x
- RTTm
- (7RTTs + RTTm) / 8
### RTTd
- x
- RTTm / 2
- (3RTTd + |RTTs - RTTm|) / 4
### RTO
- x
- RTTs + 4RTTd
## RTO 계산 예시
처음 보내는 RTO는 default값으로 설정한다.
RTO는 Sender가 측정한다.
**RTTd 측정시 새로 계산한 RTTs를 활용한다.**
1. 첫 번째 데이터 세그먼트가 전송될 때 새로운 RTT 측정이 시작된다. 두 번째 데이터 세그먼트에 대해서는 이미 측정이 진행 중이므로 새로운 RTT 측정이 시작되지 않는다. 마지막 ACK 세그먼트의 도착은 다음 RTTM 값을 계산하는 데 사용된다. 비록 마지막 ACK 세그먼트가 두 데이터 세그먼트 모두를 확인하지만(누적 확인), 그 도착은 첫 번째 세그먼트에 대한 RTTM 값을 확정 짓는다. 이 변수들의 값은 이제 다음과 같다.
## Karn's Algorithm
![[Pasted image 20241030153740.png|400]]
(a)에서 original transmission이 사라졌음
(b)에서는 Retransmission이 사라졌음
a상황인지 b상황인지 sender는 구별할 수 없다.
따라서 이 case에서는 재전송에 대한 measurement값이 오류가 발생할 수 있어 Karn's Algorithm을 사용한다. (한 번 잘못된 측정 값이 이후에도 계속 영향을 끼칠 수 있으므로)

• 재전송된 세그먼트에 대해서는 RTT를 업데이트하지 않는다.
• 재전송되지 않은 세그먼트에 대한 ACK를 받은 후에만 RTT 측정을 다시 시작한다.

## Options
TCP 헤더는 최대 선택적 정보를 가질 수 있는데, 추가 정보를 전달하거나 다른 옵션을 정렬하는 데 사용된다.
![[Pasted image 20241030154853.png|400]]
Single-byte : 땜빵용 (이정도만 알면 충분, 하나는 앞에 붙고 하나는 뒤에 붙는거)
중요한건 Multiple-byte
## End-of-option
![[Pasted image 20241030154916.png|500]]
한 개만 쓰일 수 있다.
## No-operation option
![[Pasted image 20241030155055.png|500]]
여러개 쓰일 수 있다.
## Maximum-segment-size option
![[Pasted image 20241030155125.png|500]]
kind : option 번호
Length: 전체 option 길이
MSS
## Window-scale-factor option
![[Pasted image 20241030155157.png|500]]
rwnd에 2의 n승만큼 더 보내기 위해 Scale factor에 n을 넣는다 (따라서 표현할 수 있는 값이 더 커지게 된다.)
이는 connection set up할 때만 정할 수 있고, 중간에 변할 수는 없다.

![[Pasted image 20241030155707.png|500]]
혼잡이 없어 cwnd는 계속 늘어나고 rwnd가 제한이 되는 상황
16bit를 rwnd를 사용하기 위해 잡아놨으므로 64K byte만큼의 데이터를 전송할 수 있다.
따라서 512Kbps의 Throughput을 가질 수 있다.
최대 0.5Mbps밖에 전송이 되지 않으므로 Window-scale-factor Option을 사용하여 Throughput을 증가시킬 수 있다.
## Timestamp option
![[Pasted image 20241030160801.png|500]]


![[Pasted image 20241030160812.png|600]]
출발시간을 헤더에 포함해 보내고 ACK에서 받았던 출발시간을 보내서 RTT를 구한다 (Timestamp option)
또한 타임스탬프는 PAWS(Protection Against Wrapped Sequence number)로 활용할 수도 있다. 
(만약 sequence number가 길어서 초과하게 되면 sequence number가 초과하더라도 0부터 시작하고 쭉 쓰는데 timestamp option을 활용하여 packet에 출발시간이 있어서 그것으로 구분한다.)

## SACK (selective ack)
cumulative ack의 단점
packet loss가 생겼을 때 detail한 상황을 말할 수 없다.
=> 따라서 단점이 생겼을 때 selective ack을 사용한다.
![[Pasted image 20241030161242.png|500]]
4: 서로 set-up때 SACK option을 쓰자고 협의하는 것
5.: 상대방이 4를 agree하면 4를 쓸 수 있다.

![[Pasted image 20241030162102.png|500]]
상대방이 못 받은 패킷만 보내준다.

![[Pasted image 20241030162300.png|500]]
그냥 이런게 있다...만 알기
중복된 패킷에 관한 정보도 같이 헤더에 실어서 보낼 수 있다.

SACK을 왜 쓰는지 -=> cummulative ack의 단점을 보완하기 위해서
