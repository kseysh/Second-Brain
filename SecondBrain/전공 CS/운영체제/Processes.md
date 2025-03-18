# Process Concepts
- 프로세스 = 실행 중인 프로그램
- 프로세스 = 특정한 process state의 execution stream이다.
- process state (context)
	- 프로세스가 실행되는데 관여하는 모든 것들
		- Memory context
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
#### Linux내의 PCB
보기만 하자 c에서는 PCB를 task라고 부른다.
![[Pasted image 20250318163930.png|300]]
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

Q. 모든 프로세스가 Job queue에도, Ready queue에도 보관되는 것인지?
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

- `fork()`: 복제
	- 부모는 자식의 pid를 리턴받는다
	- 자식은 0을 리턴받는다.
- `exec()`: 복제 후 loading
#### [[fork & exec]] 예제
![[Pasted image 20250318164821.png|300]]
## Process Termination
프로세스가 정상적으로 종료하려면 exit을 호출해줘야 한다.
- 자식에게서 exit이 호출되면, 부모에게 자식의 종료 status를 전달한다.
- 자식의 종료가 완료되려면 부모가 wait()을 해줘야 한다.
아래 상황에서는 부모가 kill을 하는 경우도 있다.
- 자식이 너무 resource를 많이 가져간 상황
- 필요없어진 task가 자식에게 할당된 상황
- 부모가 child가 끝나기 전에 exit해버린 상황

일부 OS는 부모가 종료된 경우 자식의 존재를 허용하지 않는다 
- 프로세스가 종료되면 모든 자식도 종료되어야 합니다.
- 계단식 종료: 모든 자식, 손자 등이 종료됩니다.
- 종료는 OS에 의해 시작됩니다.

- 좀비 프로세스: child는 종료되었지만 parent가 [[wait]]을 호출하지 않은 경우
- 고아 프로세스: child가 종료되었지만 parent가 종료된 경우
	- init이 parent가 되어 종료해줌

## Multiprogram Architecture Example
많은 web brower는 싱글 프로세스로 구동된다.
### Chrome
- Browser: user interface, disk, network I/O를 관리한다.
- Renderer: 탭 하나당 하나의 렌더러로 HTML/JS를 렌더링한다.
- Plug-in: process for each type of plug in
## Inter-process Communication ([[IPC]])
Reasons for cooperating processes:
• Information sharing
• Computation speedup
• Modularity
• Convenience
cooperating process는 IPC가 필요합니다. 
ex) 공유 메모리 • 메시지 전달
### Communication Models
![[Pasted image 20250318171848.png|300]]
#### Message Passing
커널이 메시지 큐를 이용해 아래 두 함수를 이용해 소통한다.
- msgsnd()
	- 커널 공간에 메시지를 쌓는다.
- msgrcv()
	- 커널 공간에 쌓은 메시지를 읽는다.

- 실질적으로 메시지 복사가 필요하다. -> poll, select?
	- 따라서 대용량 전달이 비효율적이다 
#### Shared memory
Shared memory를 활용하여 process A,B에게 shared memory 접근 권한을 부여한다.
- 사진에서의 shared memory는 shared memory에 직접 접근할 수 있는 주소를 부여한 것. 
	- 실제 shared memory 저장공간은 kernel 내부에 있다. 
- shared memory의 데이터의 race condition에 대해서 사용자가 shared memory를 관리하여야 한다
	- 동기화 문제를 해결해야 한다.
## [[생산자와 소비자 문제]]
#### shared data
```c
#define BUFFER_SIZE 10
typedef struct {
	. . .
} item;
item buffer[BUFFER_SIZE];
int in = 0;
int out = 0;
```
#### Producer code
```c
item next_produced;
while (true) {
	/* produce an item in next produced */
	while (((in + 1) % BUFFER_SIZE) == out){ // 하나를 더 넣었는데, out과 같다는 것은 빈공간이 없다라는 뜻
		; /* do nothing */
	}
	buffer[in] = next_produced; // 빈 공간이 있다면 buffer에 생성한 것을 넣는다.
	in = (in + 1) % BUFFER_SIZE;
}
```
#### Consumer code
```c
item next_consumed;
while (true) {
	while (in == out){ // 읽을 것이 없음
		; /* do nothing */
	}
	next_consumed = buffer[out];
	out = (out + 1) % BUFFER_SIZE;
	/* consume the item in next consumed */
}
```