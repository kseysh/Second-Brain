# CPU [[Scheduler]]
- CPU scheduling 결정을 해야하는 상황
- 선점 스케쥴링
	- running state -> waiting state
	- running state -> terminate
- 비선점 스케쥴링
	- running state -> ready state
	- waiting state -> ready state

- policy(scheduling policy)와 mechanism(dispatcher)의 분리
![[Pasted image 20250325172314.png|300]]
# Dispatcher
Dispatcher module은 CPU 스케쥴러가 선택한 프로세스에 CPU 제어권을 넘겨준다
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

time quantum이 100ms인 RR이라면,
P1:  P1은 1ms 돌고 100ms를 기다린다. => IO 프로세스가 1/10의 시간으로 일한다
P2: 