# CPU [[Scheduler]]
- CPU scheduling 결정을 해야하는 상황
- 비선점 스케쥴링
	- running state -> waiting state
	- running state -> terminate
	- 돌고 있는게 waiting이거나 종료될때까지 기다려줌
- 선점 스케쥴링
	- running state -> ready state
	- waiting state -> ready state
	- 돌던걸 멈춰버렸으니 ready가 됨

- policy(scheduling policy)와 mechanism(dispatcher)의 분리
![[Pasted image 20250325172314.png|300]]
# Dispatcher
Dispatcher module은 CPU 스케쥴러가 선택한 프로세스에 CPU 제어권을 넘겨준다
• Context Switching
• user mode로 전환
• 해당 프로그램을 다시 시작하기 위해 사용자 프로그램의 적절한 위치로 jump
- Dispatcher가 control을 유지하는 방식
	- Non-preemptive 방식
		- 프로세스가 스스로 Dispatcher를 깨우는 방법
	- Preemptive 방식
		- Dispatcher에게 time slice마다 깨워달라 하는 법
OS가 제어권을 되찾는 경우
- Trap and Faults (사용자 프로세스 내부에서 발생하는 이벤트)
	- System call
	- Floating point exception
	- Page faults(메모리에 내가 원하는 데이터가 없어서 기다리는 것)
- Interrupts (사용자 프로세스 외부에서 발생하는 이벤트)
	- 터미널에서 문자 입력
	- 디스크 전송 완료
### Context switch mechanism
디스패처는 상태를 저장하고 복원하기 위해 다음 프로세스가 변경할 수 있거나 변경할 가능성이 있는 모든 정보(context)를 저장해둔다.
- Program Counter
- Processor Status Word (PSW)
- 레지스터
- 전체 메모리를 저장하지 않고, 일부 메모리를 디스크에 저장하는 방식을 사용
	- 메모리 공간이 없을 때, 일부를 디스크에 저장한다.
# Scheduling Policies
## 스케쥴링 기준
- CPU utilization
	- CPU를 가능한 빠르게 유지한다.
- Throughput
	- 시간 당 수행되는 일의 개수
- Turnaround time
	- 일이 끝날 때까지 시간이 얼마나 걸리는가
- Waiting time
	- ready queue에 들어가고 나서 얼마나 기다리고 난 후 실행되는가
- Response time
	- 첫 번째 반응이 올 때까지의 시간
## First-Come, First-Served (FCFS)
먼저 온 것이 먼저 실행되는 것 (FIFO)
끝나거나 I/O burst가 오기 전까지 실행한다.
### 장점
구현이 쉽다
### 단점
한 프로세스가 CPU를 독점할 수 있다.
#### 해결
time slice마다 context switching을 진행한다. (RR)
### example
![[Pasted image 20250327165432.png|300]]
![[Pasted image 20250327165444.png|300]]
- convoy effect - 짧은 프로세스가 긴 프로세스 뒤에 와서 waiting time이 늘어나는 것
## Shortest Job First (SJF)
CPU burst가 짧은 것을 먼저 수행한다
- Non-preemptive
	- CPU가 프로세스에 할당되면, CPU burst가 완료될 때까지 선점될 수 없다.
- Preemptive
	- 현재 실행 프로세스의 남은 시간보다 짧은 CPU burst length로 새 프로세스가 도착하면 선점합니다. 
	- Shortest Remaining Time First (SRTF) or Shortest Time to Completion First(STCF)라고도 불린다.
### 장점
가장 바람직한 방식이다.
### 단점
CPU burst length를 측정하기 어려움
따라서, 실제로 구현하지 못하는 방식임
#### example
![[Pasted image 20250327171354.png|300]]
![[Pasted image 20250327171407.png|300]]
time slice와 context switching은 1000배 차이이므로 context switching 비용은 크게 고려하지 않아도 된다.
### CPU burst time 예측
예측 된 값 중 가장 짧은 프로세스를 선택하는 것.
예측하는 값은 과거의 값을 사용한다.

𝑡<sub>𝑛</sub>: n<sub>th</sub> CPU burst의 실제 값
𝜏<sub>𝑛+1</sub>: 다음 CPU burt의 예측 값
𝛼: 실제 값을 얼마나 높게 평가할 것인지
𝜏<sub>𝑛+1</sub> = 𝛼𝑡<sub>𝑛</sub> + (1 − 𝛼)𝜏<sub>𝑛</sub>
𝛼 = 0 이면, 최근 값을 count하지 않는다.
𝛼 = 0 이면, 마지막 CPU burst만 count한다.
![[Pasted image 20250327173201.png|150]]
풀면 이렇게 되는데, 이렇게 점점 옛날 값은 weight이 작아지게 된다.
### 실제 잘 사용하지는 않는다.
history를 보는 것이 부정확한 경우가 있기 때문
application마다 𝛼값이 다르기도 하기 때문에, 적용하기 어려움
## Round Robin (RR)
time quantum/slice라고 불리는 시간을 프로세스가 할당받음
시간이 끝나면, process는 preempted되고 ready queue에 마지막에 넣는다.
- q가 크면
	- 프로세스가 CPU를 독점한다
- q가 작으면
	- context switching이 많이 일어난다.
![[Pasted image 20250327173943.png|300]]
준비 대기열에 n개의 프로세스가 있고 time quantum이 q인 경우, 
• 각 프로세스는 한 번에 최대 q 시간 단위의로 CPU 시간의 1/n을 얻는다. 
• (n-1)q 시간 단위 이상 기다리는 프로세스는 없다. 
### 단점
두 개 프로세스, cpu burst 100ms, time quantum 10ms라면
=> A의 waiting time 90ms, B의 waiting time 100ms이므로, Average wait time = 95ms가 되어버린다.
• 라운드 로빈은 fair하지만, 균일하게 비효율적이다
#### example
P1: 1ms 돌고, 10ms를 IO를 위해 기다린다
P2: No waiting, run continuously

time quantum이 100ms인 RR이라면 (너무 크다면)
P1:  P1은 1ms 돌고 100ms를 기다린다. => IO 프로세스가 1/10의 시간으로 일한다

time quantum이 1ms인 RR이라면 (너무 작다면)
P1:  P2는 CPU burst가 긴 Job이 interrupt로 인해 손해를 볼 수 있다.
## Priority Scheduling
![[Pasted image 20250401165522.png|300]]
낮은 Integer를 가진 Job이 우선 순위가 높다.
- Preemptive
- Non-preemptive
의 방식이 있다.
### 단점
우선순위가 낮은 프로세스는 실행되지 않을 수 있다.
#### 해결책
프로세스의 우선순위를 시간이 지남에 따라 증가시킨다.
## Multilevel Queue
![[Pasted image 20250401170520.png|250]]
Ready queue를 partitioning하여 큐를 분리한다.
- foreground (interactive)
- background (batch)
각 큐마다 다른 스케쥴링 방법론을 적용한다.
큐 사이에서도 스케쥴링이 필요하다.
- Fixed priority scheduling
	- 기아 현상이 발생할 수 있음
- Round Robin
### 문제
처음에 어떤 큐에 넣느냐가 지속적인 스케쥴링에 영향을 줄 수 있다 (계속 특정 우선 순위에 있는 큐에 있어야 하므로)
## Multilevel Feedback Queue
피드백을 줘서 프로세스가 큐 간 이동할 수 있도록 한다.
#### example
![[Pasted image 20250401171011.png|200]]
큐 간의 스케쥴링은 priority로 움직이고, 큐 내부에서는 RR을 사용한다. (우선 순위가 높은 큐가 cpu burst가 짧을 것이므로 짧은 time slice를 가짐)
만약 IO없이 time slice를 모두 사용했다면, priority를 감소시킨다. 
=> 이렇게 하면 자연스레 CPU burst가 짧은 프로세스는 우선순위가 높게, 긴 프로세스는 우선순위가 낮게 분포된다.

P1: 1ms 돌고, 10ms IO를 기다리는 프로세스
P2: 계속 도는 프로세스
![[Pasted image 20250401171353.png|200]]
# Linux Scheduling
### Completely Fair Scheduler (CFS)
- 각각의 프로세스가 priority(nice value)를 가짐
- 고정 시간 할당에 기반한 quantum보다는 CPU time의 비율에 기반함 (quantum이 process마다 다른 느낌)
- 두 개의 스케쥴링 클래스가 포함된다.
	- default
	- real-time
- Quantum은 -20~19의 nice value로 계산된다.
- 작업이 한 번 이상 실행되어야 하는 시간 간격인 target latency를 계산한다
	- active task의 수가 증가하면 target latency도 증가한다.
- CFS 스케줄러는 작업당 virtual run time(vruntime)을 유지한다 
	- 작업의 우선 순위에 따라 decay factor와 연관되어 - 우선 순위가 낮을수록 decay rate가 높다 
- 실행할 다음 작업을 결정하기 위해 스케줄러는 vruntime이 가장 낮은 task를 선택한다
	- vruntime은 실제 실행시간이 아니라 중요도가 반영된 실행시간이므로 vruntime이라고 한다.
	- `vruntime += 실행시간 x ( 1024 / weight )`
![[Pasted image 20250401174108.png|300]]
Normal : Nice value
Real-Time: 실시간성이 중요한 프로세스의 우선순위 설정시 사용, 따로 관리가 된다.
#### example
Target latency 10ms 돈 이후의 vruntime을 보여주는 것
![[Pasted image 20250401173421.png|400]]
`task의 time slice = target latency x ( task의 weight / sum(weight) )`
vruntime = W<sub>0</sub>/W<sub>p</sub> x Time slice
nice value, weight은 시스템에서 제공하는 fixed value다
W<sub>0</sub>/W<sub>p</sub> = 1024/weight
Nice value가 0이면, 가상 런타임 = 실제 런타임이다 (Nice value가 0인 weight이 1024라서)
# Advanced Topic of Scheduling
## Multiple-Processor Scheduling
CPU 스케줄링은 여러 개의 CPU가 있을 때 더 복잡해진다.
- 다중 프로세서 내에서 동일한 코어를 사용하는 경우
- `Asymmetric multiprocessing`: 하나의 프로세서만 시스템 데이터 구조에 접근하여 데이터 공유에 대한 필요를 줄임
- `Symmetric multiprocessing, SMP`: 각 프로세서가 자체적으로 스케줄링을 수행하며, 모든 프로세스는 공통의 준비 큐에 있거나 각자 고유한 준비 큐를 가질 수 있음
	- 현재 가장 일반적인 방식
### SMP에서 ready queue 관리 방식
- Common ready queue
	- 모든 스레드가 하나의 common ready queue에 존재할 수 있음
	- 코어 수가 많으면 동기화 문제가 발생할 수 있음
- Per-core ready queue
	- 각 프로세서가 자신만의 독립된 스레드 큐를 가질 수 있음
	- 코어마다 load가 불균형할 가능성이 있음
	- cache hit rate가 늘어남 (코어에 캐시가 저장되므로)
![[Pasted image 20250403164152.png|300]]
### Load Balancing
SMP 환경에서는 운영체제가 모든 CPU에 고르게 작업을 배분해야 효율적임
• `Push migration` : 주기적으로 태스크가 각 프로세서의 부하를 확인하고, 과부하된 CPU에서 다른 CPU로 태스크를 옮김
• `Pull migration`: Idle 상태의 프로세서가 바쁜 프로세서로부터 대기 중인 태스크를 가져옴
## Real-Time CPU Scheduling
Deadline안에 task를 완수해야하는 시스템에 사용되는 스케줄링 방식
• Soft real-time systems: 중요한 실시간 프로세스가 언제 스케줄될지에 대한 보장이 없음
• Hard real-time systems: Task가 반드시 정해진 기한 내에 처리되어야 함

- 성능에 영향을 미치는 두 가지 지연(latency)
1. Interrupt latency: 인터럽트가 도착한 시점부터 ISR(인터럽트 서비스 루틴)이 시작될 때까지의 시간
2. Dispatch latency: Context Switching에 걸리는 시간
![[Pasted image 20250408155859.png|200]]
### Priority-based Scheduling
실시간 스케줄링을 위해서는 preemptive, priority-based scheduling을 지원해야 함
-	하지만 이는 soft real-time만 보장할 수 있음
-	hard real-time을 위해서는 deadline을 충족시킬 수 있는 기능이 추가로 필요함

실시간 프로세스의 새로운 특성
![[Pasted image 20250403165433.png|300]]
- 일정한 시간 간격으로 task가 일어나 CPU를 필요로 한다고 가정
	- 매개변수: 처리 시간 t, 마감 기한 d, 주기 p
	- 조건: 0 ≤ t ≤ d ≤ p
	- 주기적인 태스크의 실행률(rate)은 1/p
### Rate Monotic Scheduling
•	우선순위는 주기의 역수에 따라 부여됨
•	짧은 주기 → 높은 우선순위
•	긴 주기 → 낮은 우선순위
#### example
•	P1: p = 50, t = 20, d = 다음 주기 시작 시점
•	P2: p = 100, t = 35, d = 다음 주기 시작 시점
![[Pasted image 20250403165734.png|400]]
P1 CPU utilization = 20/50
P2 CPU utilization = 35/100

•	P1: p = 50, t = 25, d = 다음 주기 시작 시점
•	P2: p = 80, t = 35, d = 다음 주기 시작 시점
![[Pasted image 20250403165746.png|400]]
P1 CPU utilization = 25/50
P2 CPU utilization = 35/80

#### Earliest Deadline First Scheduling (EDF)
CPU_util의 합이 1보다 작으면 항상 만족한다.
•	우선순위는 마감 기한에 따라 부여됨
•	기한이 빠를수록 → 우선순위 높음
•	기한이 느릴수록 → 우선순위 낮음
##### example
•	P1: p = 50, t = 25, d = 다음 주기 시작 시점
•	P2: p = 80, t = 35, d = 다음 주기 시작 시점
![[Pasted image 20250403165831.png|400]]