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
	- 


