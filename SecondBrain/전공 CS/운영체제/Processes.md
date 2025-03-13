# Process Concepts
- 실행 중인 프로그램
- 특정한 process state의 execution stream이다.
- process state (context)
	- 프로세스가 실행되는데 관여하는 모든 것들
		- Memort context
			- code, data, stack, heap
		- Hardware context
			- Program counter, CPU register, I/O register
		- System context
			- process table, open file table, page table
- execution stream
	- 명령어가 실행되는 흐름
### Multiprogramming vs. multiprocessing
[[멀티태스킹, 멀티프로세스, 멀티스레딩]]
- Multiprogramming
	- 메모리에서 여러 프로세스가 동작하는 것
	- 싱글코어여도 메모리에 여러개를 올려둘 수는 있음
- Multiprocessing
	- 여러개의 프로세스들이 같은 시간에 함께 동작하는 것
	- 따라서 CPU가 여러개 있어야 함
### Process Control Block (PCB)
![[Pasted image 20250313165137.png|100]]
- 멀티 프로그래밍 사용시 OS가 프로세스들의 정보를 저장해두는 것
- 각 프로세스 단위로 저장되는 정보
	- 프로세스 상태
	- Program counter
	- Registers
	- Scheduling information
	- Memory management information
	- I/O status information
- 시스템 전반으로 저장되는 정보
	- Process table
# Process State
![[OS 프로세스 상태]]
## State Transition
![[Pasted image 20250313170123.png|300]]
![[Pasted image 20250313170257.png|300]]
### Ready queue
ready 상태인 프로세스를 모아둔 큐
### Device queue
I/O로 인해 waiting하는 프로세스를 모아둔 큐
### Job queue
시스템에서 모든 프로세스를 Job queue에 보관한다.
## [[scheduler]]

### Short-term scheduler (CPU scheduler)
밀리초 단위로 자주 호출되기 때문에 빠르게 동작해야 한다
### Long-term scheduler (job scheduler)
- 어떤 프로세스를 Job queue에 넣을 것인지 선택하는 역할을 한다 (메모리에 너무 많은 process가 올라가면 안되므로)
- 초 또는 분 단위로 호출되기 때문에 상대적으로 느려도 괜찮다.
- multi programming 수준을 조절하는 역할을 한다.
- 적절한 프로세스 균형을 유지하는 것을 목표로 한다.
	- cpu bound 작업은 I/O bound 작업과 mix하는 것이 좋으니 그런 방식으로 조합한다.
## CPU Switch From Process to Process
[[컨텍스트 스위칭]]
![[Pasted image 20250313171935.png|300]]
프로세스의 Context들은 PCB에 저장된다.
## Threads
프로세스당 여러 개의 PC를 고려할 수 있음
process state를 하나로 두고, execution stream을 여러개 두는 것
# Process Creation and Termiation
## Process Creation
프로세스 생성 두 가지 방법
### build from scratch
- loading 과정: code, data를 program file에서 읽어서 memory에 적재 (stack, heap은 돌아가면서 생기므로)
- create empty stack (메모리에 자리만 잡아준다.)
- PCB 만들기
- 해당 PCB를 ready queue에 넣기
### 프로세스 복제 (리눅스에서 사용되는 방법)
- 현재 state 저장(PC, register)
- memory context 복사
- PCB 복사 (pid, parent, child만 변경)
- 해당 PCB -> ready queue로 복사
![[Pasted image 20250313173540.png|300]]
init process는 build from scratch 방식으로 만들고, 이후 프로세스는 모두 복제로 생성된다.
이는 서로 다른 프로세스 간의 소통 창구를 만들기 위해 이렇게 설계되었다.

`fork()`: 복제
`exec()`: 복제 후 loading

	pid 0 스케쥴러 프로세스는 뭐지???
	얘도 build from scratch?
