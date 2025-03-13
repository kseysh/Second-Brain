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
Clock Cycle은 하드웨어적으로 정해지는 것
### Ex) CPU Time 10 -> 6으로 줄이기
2GHz Clock과 10s CPU Time을 6s로 줄이고 싶다면 
하지만 그로 인해 clock cycle이 1.2배 올라간다면
![[Pasted image 20250306130742.png|400]]
Clock Rate를 4GHz로 올리면 된다는 것을 알 수 있다.
## Instruction Count and CPI(Cycle per Instruction)
어떤 Instruction을 사용하느냐에 따라 Clock Cycle이 달라진다.
![[Pasted image 20250306130901.png|400]]
Clock Cycle을 줄이기 위해서는 Instruction Count, CPI를 줄이면 된다.
- Instruction Count for a program
	- program 코드나 ISA와 compiler에 따라 달라진다.
- CPI 시간
	- CPU 하드웨어에 따라 달라진다.
	- instruction에 따라 CPI도 다르다
### CPI Example
![[Pasted image 20250311120829.png|350]]
A: 4GHz, 한 instruction이 2 cycle만에 동작한다
B: 2GHz, 한 instruction이 1.2 cycle만에 동작한다
CPI는 낮을 수록 좋다(적은 cycle로 처리할 수 있다는 것이므로)
1ns에 1GHz이다.
ps = 1/1000ns
A가 B보다 1.2배 빠르다
## CPI in More Detail
![[Pasted image 20250311122051.png|300]]
CPI는 각각의 Instruction마다 다르다
CPI는 보통 Instruction의 평균을 뜻한다.
### CPI Example
![[Pasted image 20250311122134.png|400]]
CPI를 계산할 수 있도록 알아두자

## CPU Time 계산
![[Pasted image 20250311122716.png|300]]
- 수행시간에 영향을 미치는 것
	- algorithm: insturction count와 CPI에 영향을 줄 수 있다. (영향이 크게 작용할 수 있다.)
	- Programming language: insturction count와 CPI에 영향을 줄 수 있다.
	- Compiler: insturction count와 CPI에 영향을 줄 수 있다.
	- ISA: insturction count와 CPI, T<sub>c</sub>(clock cycle time)에 영향을 줄 수 있다.
## Power (전력)
![[Pasted image 20250311123238.png|300]]
Capacitive load: 부하 = 회로가 얼마나 큰지
Voltage: 전압
Frequency: 얼마나 빠르게 전기를 보낼지
Reducing power 수식 안 봐도 됨
## Multiprocessors
하나의 칩에 여러개의 processor를 두는 방식
- explicitly parallel programming을 필요로 한다
	- Compare with instruction level parallelism(ILP)
		- 하드웨어가 여러 개의 instruction을 한 번에 처리한다
이런 Benchmark가 있다 정도만 알기
## Amdahl's Law
![[Pasted image 20250311130041.png|400]]
- 곱셈만 빠르게 해서는 5배의 개선율은 불가능하다.
- 따라서, 자주 사용하는 case를 빠르게 해야 개선이 유의미할 수 있다
## Low Power at Idle
![[Pasted image 20250311130619.png|150]]
power는 초반에 급격하게 오른다.
power는 부하에 따라 선형적으로 오르지 않는다.
보통 컴퓨터는 Idle 상황에 놓여있는 경우가 많다. 심지어 Google Data Center도 마찬가지다
따라서 전력 효율을 줄이고자 할 때, 100% load 상황에서의 전력 효율이 아니라 Idle 상황에서의 전력 효율을 줄이는 것이 유의미하다.

## MIPS as a Performance Metric
MIPS: Millions of Instructions Per Second
- computer 사이의 ISA의 차이 => 필요 instruction 수가 달라진다.
- intructions 간의 복잡도의 차이
따라서 MIPS로 성능 측정을 하면 안된다.

ㅑ## 결론
- ISA - hardware / software interface
- Execution Time으로 성능 측정을 해야 한다
- Power는 limiting factor
	- 병렬성을 활용하여 성능을 향상시킨다.
이 장을 달달 외울 필요까지는 없다