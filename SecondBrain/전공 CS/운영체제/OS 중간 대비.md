###### Batch 모니터 사용방법
카드 입출력 장치에 적재
작업 배치를 자기 테이프에 읽어들임
주기계에서 계산 수행 및 결과를 테이프로 저장
입출력 장치에서 테이프에서 출력 결과 인쇄
###### Batch Monitor가 job-to-job transition을 빠르게 할 수 있었던 방법
- SPOOL
	- Job을 카드에서 디스크로 미리 읽어두고, 출력도 디스크에 대기시켜두었다가 나중에 프린터로 출력하는 방법
	- 이로 인해 입출력 작업을 기다리지 않고 계산에 집중할 수 있어 Job-to-Job transition에서 발생하는 IO 성능 제약 해소
	- 작업을 큐에 넣고 비동기 처리하기 위해 사용
###### Buffering이란
입출력 장치와 CPU 사이에 데이터를 임시 저장하여 입출력 속도의 차이를 완화하는 방법
###### Interrupt란
입출력 작업이 완료되었을 때, CPU에 신호를 보내 CPU가 불필요하게 대기하지 않도록 하는 방법
###### Multi-programmed Batch Monitor란
- 여러 사용자가 시스템을 공유
- memory protection 및 reloacation 기능이 운영체제에 추가
- Concurrent Programming이 필수로 등장함
###### 60-90년대 OS 발전
파일 시스템 추가
Interactive time-sharing OS
Response time, protection 중요해짐
###### 90~ OS 발전
- Internet acess가 built in
- Multi tasking 중요
- Multimedia support
###### System bus란?
computer system에서 주요한 컴포넌트들의 연결
###### Bus component
- Bus arbiter
	- 버스를 관리한다.
- Bus master
	- 요청을 보낸다.
	- ex) cpu
- Bus slave
	- 요청을 받아서 그에 맞는 행위를 한다
	- ex) memory
###### IO 컨트롤러의 주요 레지스터
•	데이터 레지스터 (입력, 출력)
•	제어 레지스터 (제어, 상태)
###### IO 출력 작업 예시
CPU에 의해 시작됨
1.	상태 레지스터를 읽어 출력 레지스터가 사용 가능한지 확인
2.	사용 가능하면 출력 레지스터로 데이터 이동, 제어 레지스터에 출력 명령 전달
3.	사용 불가능하면 이 과정을 반복하거나 대기
###### Polling I/O vs. Interrupt-driven I/O
- Polling I/O: CPU가 I/O 상태 레지스터를 계속 확인하여 작업이 완료되었는지 검사
- Interrupt-driven I/O: I/O 컨트롤러가 작업 완료 시 인터럽트를 발생하여 CPU에 알림
###### Memory-mapped I/O vs. Port-mapped I/O
- Memory-mapped I/O: 메모리와 I/O를 동일한 주소 공간에서 관리
- Port-mapped I/O: I/O 장치를 위한 별도의 주소 공간을 사용
###### DMA란?
Direct Memory Access
- **장치 컨트롤러가 CPU의 개입 없이 버퍼 스토리지와 메인 메모리 간에 데이터 블록을 직접 전송할 수 있도록 하는 장치**
- I/O 작업만 담당하는 CPU라고 생각하자
- 큰 메모리 Access를 한 번에 하는 것이 어렵기 때문에 DMA를 활용해서 옮긴다.
- Keyboard 입력 같은 경우는 데이터의 크기가 크지 않아 CPU에 그냥 인터럽트를 보낸다.
- 한 블록당 한 번의 인터럽트만 발생 (바이트당 한 번의 인터럽트 발생과 비교하여 효율적)
###### Trap, Exception
•	소프트웨어에 의해 생성된 인터럽트
•	에러 또는 사용자 요청(예: 오류, 강제 종료 등)에 의해 발생
###### PC란
다음 실행해야 하는 명령어의 주소를 담는 것
###### Interrupt가 들어올 때 CPU의 행동
- 현재 수행중인 instruction을 마무리 시키고, 현재 프로그램을 중지한다
- 다음에 수행해야할 PC와 현재 레지스터 값들을 저장해둔다
- Interrupt vector에서 어떤 코드를 실행해야 할지에 대한 ISR 주소를 얻어온다 
- ISR로 이동한다.
- ISR이 끝나면, 원래 하던 저장되어 있던 프로그램으로 돌아간다.
###### Important principle in designing OS
	역할과 구현의 분리
- Policy(구현): 어떤 것을 할 것인가?
- Mechanism(역할): 어떻게 동작할 것인가?
###### MS-Dos Architecture
모놀리식 구조여서 한 가지만 변경하더라도 전부를 컴파일해야하며, 하나가 오류가 나더라도 모두가 죽는다.
###### Layered-Approach란?
가장 안쪽은 하드웨어
가장 바깥쪽은 유저 인터페이스이다.
Layer간을 뛰어넘지 않고, 한 단계씩에만 영향을 끼칠 수 있도록 한다.
###### Microkernel (Mach)의 정의
중요하지 않은 컴포넌트는 커널에서 다 빼고 유저 레벨 프로그램에서 작성하도록 하는 방법
message 전달을 통해 사용자 모듈간 통신을 한다.
###### Microkernel (Mach)의 장점
- Kernel size가 작아진다
- 운영체제를 새로운 아키텍처로 이식하기 쉽다
- 커널 모드에서 실행되는 코드가 줄어들어 안정적이다
- 보안성이 향상된다.
###### Microkernel (Mach)의 단점
- user space와 kernel space간의 통신으로 인해 성능 오버헤드가 발생한다.
###### Program란?
디스크에 저장된 실행 가능한 파일
###### Process란?
실행 중인 프로그램을 뜻하며, 특정한 process state의 execution stream
###### process state(context)란?
- 프로세스가 실행되는데 관여하는 모든 것들
	- Memory context
		- code, data, stack, heap
	- Hardware context
		- Program counter, CPU register, I/O register
	- System context
		- process table, open file table, page table
###### Multiprogramming이란?
- 메모리에서 여러 프로세스가 동작하는 것
- 싱글코어여도 메모리에 여러개를 올려둘 수는 있음
- 즉, concurrency와 관련된 개념
###### Multiprocessing이란?
- 여러개의 프로세스들이 같은 시간에 함께 동작하는 것
- 따라서 CPU가 여러개 있어야 함
- 즉, parallelism과 관련된 개념
###### PCB란?
Process Control Block으로
- 멀티 프로그래밍 사용시 OS가 프로세스들의 정보를 저장해두는 것
###### PCB에 저장되는 값
- 프로세스 상태
- Program counter
- Registers
- Scheduling information
- Memory management information
- I/O status information
###### Process state
- new
- running
- waiting
- ready
- terminated
###### Device queue란
I/O로 인해 waiting하는 프로세스를 모아둔 큐
###### Job queue란
시스템에서 모든 프로세스가 저장되는 큐
사용자가 프로그램 실행 시 Job queue에 들어가고, OS가 메모리 공간을 할당하면 Ready queue에 들어감
######  Short term scheduler란?
다음에 실행해야 할 프로세스를 선택하고 CPU를 할당한다.
밀리초 단위로 자주 호출되기 때문에 빠르게 동작해야 한다
###### Long term scheduler란?
- *어떤 프로세스를 Job queue에 넣을 것인지 선택*하는 역할을 한다 (메모리에 너무 많은 process가 올라가면 안되므로)
- 초 또는 분 단위로 호출되기 때문에 상대적으로 느려도 괜찮다.
- multi programming 수준을 조절하는 역할을 한다.
- *적절한 프로세스 균형을 유지하는 것을 목표*로 한다.
	- cpu bound 작업은 I/O bound 작업과 mix하는 것이 좋으니 그런 방식으로 조합한다.
###### build from scratch 방식
- loading 과정: code, data를 program file에서 읽어서 memory에 적재 (stack, heap은 실행하면서 생기므로)
- create empty stack (메모리에 자리만 잡아준다.)
- PCB 만들기
- 해당 PCB를 ready queue에 넣기
###### 프로세스 복제
- 현재 state 저장(PC, register)
- *memory context 복사*
- PCB 복사 (pid, parent, child만 변경)
- 해당 PCB -> ready queue로 복사
###### Message passing 방식과 단점
커널이 메시지 큐를 이용해 아래 두 함수를 이용해 소통한다.
- msgsnd()
	- 커널 공간에 메시지를 쌓는다.
- msgrcv()
	- 커널 공간에 쌓은 메시지를 읽는다.
- 실질적으로 메시지 복사가 필요하다.
	- 따라서 대용량 전달이 비효율적이다 
###### Shared memory 방식과 단점
Shared memory를 활용하여 process A,B에게 shared memory 접근 권한을 부여한다.
- shared memory의 데이터의 race condition에 대해서 사용자가 shared memory를 관리하여야 한다
	- 동기화 문제를 해결해야 한다.
###### 멀티 스레드의 장점
- Responsiveness
	- 프로세스의 일부(스레드)가 Block되어도 계속 실행될 수 있다.
- 자원 공유
	- 스레드는 프로세스의 자원을 공유하며, shared memory나 메시지 전달 방식보다 더 쉽게 사용할 수 있다.
	- IPC가 필요하지 않다.
- 경제적
	- 프로세스 생성/종료보다 시간이 적게 든다
	- 같은 프로세스의 thread간 변경 비용이 적다.
	- Stack 과 스레드 당 static memory의 적은 리소스를 사용한다.
- 확장성
	- multiprocessor(CPU 코어가 여러개) architecture의 장점을 가질 수 있다.
###### Parallelism Type
- Data parallelism - 데이터를 나누는 것 (하나의 일, 다른 데이터)
- Task parallelism - 일을 나누는 것 (서로 다른 일)
###### Parallelism vs Concurrency
parallelism: 물리적으로 동일한 시간에 작동하는 것 (멀티 코어)
Concurrency: 동일한 시간에 작동하는 것처럼 보이는 것
###### process에서의 두 가지 특성
- *자원 소유의 단위*
	- 프로세스 이미지를 저장하기 위한 가상 주소 공간 할당
	- 파일, I/O 장치 등 *일부 자원에 대한 제어 권한 보유*
	- 일반적으로 Process or Task라고 한다.
- *실행 흐름의 단위*
	- 제어 스레드를 가짐
	- 실행 상태 및 *dispatch 우선 순위를 가짐*
	- 프로세스 실행은 다른 프로세스와 교차되어 수행될 수 있음
	- thread라고 한다.
###### thread당 stack이 부여되는 이유
stack에는 지역 변수가 저장되는데, 각 thread마다 실행 흐름이 다르므로 각 thread마다 실행하는 함수가 다르고 따라서 stack을 따로 가진다.
###### User thread, kernel Thread란?
user thread: user api를 통해 만들어지는 thread
kernel Thread: os에서 관리하는 thread 단위, CPU 스케쥴링의 대상이 된다.
###### Many to one  방식
여러 개의 유저 스레드가 하나의 커널 스레드에 매핑되는 방식
장점
- kenel-user mode switching이 없다
- 스케쥴링을 application별로 specific하게 할 수 있다.
- OS에서 멀티스레드를 구현하지 않아도 구현할 수 있다.
단점
- 병렬성을 활용하지 못한다.
- 하나의 스레드가 Block되면, 모든 thread가 block된다.
######  One to One 방식
유저가 스레드를 하나 만들면 커널이 스레드를 하나 만드는 것
장점
- 병렬성 증가
- Blocking이 스레드 단위로 수행
- 커널 루틴이 멀티스레드를 지원함
단점
- 동일한 프로세스 내에서 스레드 전환이 커널을 거치므로 성능 저하 발생(그렇게 느리지는 않아서 단점이라 하기 뭐함)
###### Pthreads create, exit, join, yield역할
create: 새로운 스레드 생성
exit: thread 종료
join: 스레드 종료를 기다림: running -> waiting
yield: 다른 스레드가 run할 수 있도록 running -> ready상태로 전환
###### Implicit Threading이란?
프로그래머보다 compiler와 *run-time library가 thread의 생성 및 관리를 담당*하는 것
 - 주요 방식
	 - Thread Pools (스레드 풀)
    - OpenMP
    - Grand Central Dispatch
###### OpenMP란?
컴파일러가 알아서 확인하면 multithread를 생성 및 관리해줌
CPU core의 개수만큼 스레드를 생성해 병렬처리를 지원해준다
`#pragma omp parallel`
###### 스레드의 개념과 프로세스와의 차이점
프로세스 안에 스레드가 여러개 있는 것이고, 스레드는 프로세스에서 excution stream을 여러개 분리한 것
###### Dispatcher의 역할
CPU 스케쥴러가 선택한 프로세스에 CPU 제어권을 넘겨준다
• Context Switching
• user mode로 전환
• 해당 프로그램을 다시 시작하기 위해 사용자 프로그램의 적절한 위치로 jump
###### OS가 제어권을 되찾는 경우
- Trap and Faults (사용자 프로세스 내부에서 발생하는 이벤트)
	- System call
	- Floating point exception
	- Page faults(메모리에 내가 원하는 데이터가 없어서 기다리는 것)
- Interrupts (사용자 프로세스 외부에서 발생하는 이벤트)
	- 터미널에서 문자 입력
	- 디스크 전송 완료
###### Context Switching시 저장하는 정보
- Program Counter
- Processor Status Word (PSW)
- 레지스터
- 일부 메모리를 디스크에 저장
	- 메모리 공간이 없을 때, 일부를 디스크에 저장한다.
###### FCFS 장단점
장점: 구현이 쉽다
단점: 한 프로세스가 CPU를 독점할 수 있다
###### convoy effect란?
짧은 프로세스가 긴 프로세스 뒤에 와서 waiting time이 늘어나는 것
###### SJF 장단점
CPU burst가 짧은 것을 먼저 수행한다.
Shortest Remaining Time First (SRTF) or Shortest Time to Completion First(STCF)라고도 불린다.

가장 바람직한 방식이지만, CPU burst length를 측정하기 어려움
###### RR 장단점, quantum이 너무 크거나 작다면
- q가 크면
	- 프로세스가 CPU를 독점한다
- q가 작으면
	- context switching이 많이 일어난다.
공평하지만 균일하게 비효율적이다.
###### RR에서 최대 waiting time은?
준비 대기열에 n개의 프로세스가 있고 time quantum이 q인 경우, 
• 각 프로세스는 한 번에 최대 q 시간 단위의로 CPU 시간의 1/n을 얻는다. 
• (n-1)q 시간 단위 이상 기다리는 프로세스는 없다. 
###### priority scheduling의 단점 해결책
우선 순위가 낮은 프로세스는 실행되지 않을 수 있다.
프로세스의 우선순위를 시간이 지남에 따라 증가시킨다.
###### Multilevel queue
Ready queue를 partitioning하여 큐를 분리한다
각 큐마다 다른 스케쥴링 방법론을 적용한다
큐 사이에도 스케쥴링이 필요하다.
###### Multilevel queue 단점
처음에 어떤 큐에 넣느냐가 지속적인 스케쥴링에 영향을 줄 수 있다 (계속 특정 우선 순위에 있는 큐에 있어야 하므로)
###### Multilevel Feedback Queue
피드백을 줘서 프로세스가 큐 간 이동할 수 있도록 한다.

큐 간의 스케쥴링은 priority로 움직이고, 큐 내부에서는 RR을 사용한다. (우선 순위가 높은 큐가 cpu burst가 짧을 것이므로 짧은 time slice를 가짐)
만약 IO없이 time slice를 모두 사용했다면, priority를 감소시킨다. 
=> 이렇게 하면 자연스레 CPU burst가 짧은 프로세스는 우선순위가 높게, 긴 프로세스는 우선순위가 낮게 분포된다.
###### CFS
각각의 프로세스가 priority(nice value)를 가짐
- 고정 시간 할당에 기반한 quantum보다는 CPU time의 비율에 기반함
- 두 개의 스케쥴링 클래스가 포함된다.
	- default
	- real-time
- Quantum은 -20~19의 nice value로 계산된다.
- 실행할 다음 작업을 결정하기 위해 스케줄러는 가상 런타임이 가장 낮은 태스크를 선택한다
###### CFS에서 task의 time slice
`time slice = target latency x ( task의 weight / sum(weight) )`
###### CFS에서 vruntime
`vruntime += 실행시간 x ( 1024 / weight )`
###### critical section problem의 해결책이 되기 위한 조건
1. mutual exclusion (상호 배제)
    1. 한 번에 하나의 스레드만 임계 영역에서 실행할 수 있다.
2. progress (진행)
    2. 만약 임계 영역이 비어있고, 여러 스레드가 임계 영역에 들어가길 원한다면 그 중 하나는 임계 영역에 들어가서 실행할 수 있어야 한다.
3. bounded waiting (한정된 대기)
    1. 임계 영역을 무한정 기다리면 안된다.
###### OS에서의 Critical section 처리 방식 비교 preemptive vs non-preemptive
- Preemptive
	- 커널 모드에서 실행 중인 프로세스도 선점될 수 있음
	- 실제 사용하는 방식
- Non-preemptive
	- 커널 모드에서 빠져나가거나, 차단되거나, 자발적으로 CPU를 양보할 때까지 실행함
		- 커널 모드에서는 본질적으로 race condition이 없음
###### Peterson's solution code
![[Pasted image 20250408171209.png|300]]
자기가 준비되었으면, turn을 j에게 넘기고
내가 턴을 넘겼는데, j가 대기하지 않거나 j가 turn을 나에게 넘기면 내 차례
아래 두 조건 중 하나를 만족하면 while이 풀림
`flag[j]==false`: j는 critical section에 들어가고 싶지 않음
`turn != j`: j가 turn을 나한테 넘김
###### turn을 넘겨야 하는 이유
turn을 자신으로 하고 실행하면, 서로 자신 먼저 들어가려고 하므로 누가 먼저 들어가야 할지 결정할 기준이 없음 따라서 turn을 동비해서 누가 먼저 들어갈지 순서를 정하는 장치로 사용
###### peterson's solution이 세 조건을 만족하는 이유
- 상호 배제
	- `P[i]`는 `flag[j] == false`거나, `turn == i` 일때만 임계 구역에 진입함
- Progress
	- critical section 이후 `flag[i]`를 false로 하므로 progress 만족
- bounded waiting
	- 한 바퀴 돌면 항상 순서는 반대로 넘어가게 되어있음
###### peterson's solution이 현대 컴퓨터에서 조건을 만족하지 않는 이유
피터슨의 해법은 현대 컴퓨터 아키텍처에서는 항상 동작한다고 보장할 수 없음
→ 프로세서나 컴파일러가 의존성이 없는 읽기/쓰기 연산의 순서를 바꿀 수 있음
![[Pasted image 20250408171904.png|300]]
위 순서처럼 뒤바뀔 시 critical section에 둘 다 들어갈 가능성이 생긴다.
또한, turn 값도 atomic하지 않기 때문에, 두 프로세스가 다른 값으로 인식할 수도 있다.
###### TAS 코드
```c
boolean test_and_set (boolean *target) {
	boolean rv = *target;
	*target = TRUE;
	return rv;
}
```
###### TAS가 bounded waiting을 보장하지 못하는 이유
공유변수인 lock은 false로 initialized 되어 있음
```c
do {
while (test_and_set(&lock)); /* do nothing */
		/* critical section */
	lock = false;
		/* remainder section */
} while (true);
```
- lock = 1 잠김, lock = 0 열림
- 호출과 동시에 lock이 1이됨
- *하지만, 계속 한 프로세스만 들어갈 수 있어 Bounded Waiting을 보장하지 못함*
###### bounded waiting solution
```c
do {
	waiting[i] = true;
	key = 1;
	while (waiting[i] && key == 1)
		key = test_and_set(&lock)); // 내가 기다리고 있고, key가 있으면 들어와서 key를 1로 변경
	waiting[i] = false;
	
		/* critical section */
		
	j = (i + 1) % n; // 내 옆에 있는 애한테 넘겨줄거임
	while ((j != i) && !waiting[j]) // 내 옆에 있는 애가 안 기다리면
		j = (j + 1) % n;// 내 옆에 있는 애의 옆에 애를 줌
	// 기다리는 애가 있거나, 한 바퀴 다 돌았다면 while 문을 벗어날 수 있음
	if (j == i) lock = 0; // 락을 풀면 누구나 들어가도록 하는 것
	else waiting[j] = false;// 기다리는 애가 있으면 j만 들어갈 수 있도록 바꿔준다.
		/* remainder section */
} while (true);
```
처음 key를 얻는 과정: TAS를 이용해 한 명만 lock을 얻음
대기 줄이 있을 때 임계 구간을 진입하는 과정: `waiting[j]==false`로 만들어서 critical section에 들어오게 한다.
###### Mutex Lock이란?
임계 구역을 보호하기 위해서는 먼저 `acquire()`로 락을 획득하고, 작업 후 `release()`로 락을 해제해야 한다
	•	락이 사용 가능한지 여부를 나타내는 Boolean 변수 사용 (이도 atomic하게 이루어져야 함)
	•	`acquire()`와 `release()` 호출은 원자적(atomic)으로 이루어져야 한다
	•	보통 하드웨어의 원자적 명령을 이용해 구현된다
![[Pasted image 20250410163638.png|400]]
이 방법은 바쁜 대기(busy waiting)를 필요로 한다
	•	따라서 이러한 락을 스핀락(spinlock) 이라고 부른다
###### wait, signal 정의
![[Pasted image 20250410163943.png|300]]
wait p signal v
###### semaphore = 0이면?
순서 동기화 가능
![[Pasted image 20250410164235.png|300]]
###### S=1이면?
뮤텍스 락과 동일하게 동작
###### semaphore with busy waiting code
![[Pasted image 20250410164421.png|400]]
###### 위 코드에서 rel, acq이 while문 안에 있어야 하는 이유
S를 확인하는 과정과 확인하고 S를 줄이는 과정 사이에 acq()와 rel()이 필요함
이유: 저거 안 풀어주면 signal 실행이 안되어서 while문 안에 계속 있어야 함
###### semaphore with no busy waiting code
![[Pasted image 20250410164744.png|400]]
###### 위 코드에서 interrupt방식을 사용한 이유
임계구역에서의 코드가 빠르게 풀릴 수 있는 연산이기 때문
disable -> enable 사이의 활동이 짧기 때문에 interrupt로 해결함
###### block, wakeup operation
- Two operations
	- block: 해당 연산을 호출한 프로세스를 적절한 대기 큐에 넣음
	- wakeup: 대기 큐에 있는 프로세스 중 하나를 제거하여 준비 큐(ready queue)로 이동시킴
###### Q
A
