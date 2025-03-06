## Relative Performance
Performance 정의
X가 Y보다 n배 빠르다 \== ![[Pasted image 20250306125143.png|200]]
## 수행시간 측정
- Elapsed Time(경과 시간)
	- Total response time
	- 전체 성능을 볼 때 좋다
- CPU Time (우리는 컴퓨터의 성능을 볼거니까 이거 볼거임)
	- 해당 job을 실행하기 위해 실제로 cpu가 사용된 시간
	- idle된 시간은 측정하지 않음
## CPU Clocking
![[Pasted image 20250306125752.png|300]]
컴퓨터는 Clock 단위로 일한다.
Clock period : duration of a clock cycle
Clock frequency : cycles per second
ms = -3
us = -6
ns = -9
kHz = 3
MHz = 6
GHz = 9
## CPU Time
![[Pasted image 20250306130010.png|300]]
cpu time이 10초라면, 1ns가 10G번만큼 돈 것
CPU의 성능을 높이기 위해서는 CPU Time을 내리면 되므로 
CPU Clock Cycle의 횟수를 줄이거나, Clock Rate를 올리면 된다.
하지만, 둘은 trade off가 있다.
Clock Cycle을 줄이면 그만큼 하는 일이 줄어들기 때문이다.

### Ex) CPU Time 10 -> 6으로 줄이기
2GHz Clock과 10s CPU Time을 6s로 줄이고 싶다면
![[Pasted image 20250306130742.png|400]]
Clock Rate를 4GHz로 올리면 된다는 것을 알 수 있다.
## Instruction Count and CPI(Cycle per Instruction)
어떤 Instruction을 사용하느냐에 따라 Clock Cycle이 달라진다.
![[Pasted image 20250306130901.png|400]]
Clock Cycle을 줄이기 위해서는 Instruction Count, CPI를 줄이면 된다.
- Instruction Count for a prof