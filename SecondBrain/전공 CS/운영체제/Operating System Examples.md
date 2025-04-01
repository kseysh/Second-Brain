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
- 
