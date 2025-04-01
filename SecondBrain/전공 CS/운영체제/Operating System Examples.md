# Linux Scheduling
### Completely Fair Scheduler (CFS)
- 각각의 프로세스가 priority(nice value)를 가짐
- 고정 시간 할당에 기반한 quantum보다는 CPU time의 비율에 기반함
- 두 개의 스케쥴링 클래스가 포함된다.
	- default
	- real-time
- Quantum은 -20~19의 nice value로 계산된다.
- 작업이 한 번 이상 실행되어야 하는 시간 간격인 target latency를 계산한다
	- active task의 수가 증가하면 target latency도 증가한다.
- task의 time slice = target latency x ( task의 weight / sum(weight) )
- CFS 스케줄러는 작업당 virtual run time(vruntime)을 유지한다 
	- vruntime += 실행시간 x ( 1024 / weight )
	- 작업의 우선 순위에 따라 decay factor와 연관되어 - 우선 순위가 낮을수록 decay rate가 높다 
	- Nice value가 0이면, 가상 런타임 = 실제 런타임이다 (Nice value가 0인 weight이 1024라서)
- 실행할 다음 작업을 결정하기 위해 스케줄러는 가상 런타임이 가장 낮은 태스크를 선택한다
![[Pasted image 20250401174108.png|300]]
Normal : Nice value
Real-Time: 실시간성이 중요한 프로세스의 우선순위 설정시 사용, 따로 관리가 된다.
#### example
![[Pasted image 20250401173421.png|400]]
weight은 시스템에서 제공하는 fixed value다
W<sub>0</sub>/W<sub>p</sub> = 1024/weight
