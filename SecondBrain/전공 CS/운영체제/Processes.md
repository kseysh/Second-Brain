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
## 
# Process State

# Process Creation and Termiation
