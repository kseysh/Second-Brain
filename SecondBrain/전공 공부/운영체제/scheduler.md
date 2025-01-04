![[Pasted image 20250104225808.png|350]]
CPU에서 실행되기를 원하는 (ready 상태로 가기를 원하는) 스레드들이 ready queue에 저장된다.
scheduler는 이 중 CPU에서 실행될 프로세스를 선택하는 역할을 한다.
### dispatcher
스케쥴러가 선택한 프로세스를 실제로 cpu에서 실행할 수 있도록 하는 역할을 한다. 
ex) 
- context switching
- 커널 모드 -> 유저 모드 변경
- 새롭게 선택된 프로세스를 실행되어야 할 적절한 위치로 이동시키는 역할

## 스케쥴링 선점 방식
### Nonpreemptive scheduling (비선점 스케쥴링)
running 중인 프로세스가 
- IO작업으로 인한 waiting 
- 종료로 인한 terminated  
- 다른 이유로 인해 cpu를 양보하여 ready 상태로 전환
이 세 가지 경우에 대해서만 OS에서 개입을 하여 스케쥴링을 하는 것

=> 프로세스가 자발적으로 running 상태에서 빠져나가기 때문에 비선점 스케쥴링이라고 한다.

신사적(os가 프로세스가 cpu를 충분히 쓸 수 있도록 기다려준다.), 협력적(프로세스가 스스로 cpu를 양보한다.), 느린 응답성이라는 특징을 가진다.
### Preemptive scheduling (선점 스케쥴링)
비선점 스케쥴링이 하는 모든 일을 하지만, 프로세스가 cpu에서 실행이 다 끝내지 않았음에도 개입하는 경우가 있다.
ex) 
time slice가 다 끝나서 강제로 context switching을 진행하는 경우
waiting에서 ready 상태가 된 프로세스가 running 중인 프로세스보다 우선 순위가 높아 context switching을 진행하는 경우

적극적, 강제적, 빠른 응답성, 데이터 일관성 문제(이를 해결하기 위해 동기화를 진행하는 것)라는 특징을 가진다.

## 스케쥴링 알고리즘
### FCFS (First-Come, First-Served)
먼저 도착한 순서대로 처리한다.
### SJF(Shortest-Job-First)
프로세스의 다음 CPU burst가 가장 짧은 프로세스부터 실행
### SRTF (Shortest-Remaining-Time-First)
남은 CPU burst가 가장 짧은 프로세스부터 실행
### Priority
우선순위가 높은 프로세스부터 실행
### RR(Round-Robin)
time slice로 나눠진 CPU time을 번갈아가며 실행
### Multilevel queue
프로세스들을 그룹화해서 그룹마다 큐를 두는 방식
