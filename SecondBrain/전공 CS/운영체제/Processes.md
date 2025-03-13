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
![[Pasted image 20250313170123.png]|]
### Ready queue
### Device queue
### Job queue
# Process Creation and Termiation
