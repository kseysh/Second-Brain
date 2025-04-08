## [[생산자와 소비자 문제]]
### producer
![[Pasted image 20250408164951.png|300]]
### consumer
![[Pasted image 20250408165007.png|300]]
### problem
![[Pasted image 20250408165112.png|300]]
더하고 저장하는 것은 여러 instruction을 사용하므로, 중간에 다른 instruction이 실행될 수 있다.
이렇게 공유 객체에 대해 race condition이 일어날 수 있다.
## Critical Section Problem
- 시스템 내에 n개의 프로세스가 있다고 가정
- 각 프로세스는 임계 구역(Critical Section)이라는 코드 구간을 가짐
	- 이 구간에서 공통 변수 수정, 테이블 업데이트, 파일 쓰기 등을 수행할 수 있음
	- 한 프로세스가 임계 구역에 있을 때, 다른 어떤 프로세스도 그 구역에 들어가면 안 됨
- 임계 구역 문제란 이러한 상황을 해결하기 위한 프로토콜을 설계하는 것
- 각 프로세스는 임계 구역에 들어가기 전에 진입 구역(entry section)에서 허락을 요청하고, 임계 구역을 마치면 종료 구역(exit section)을 거쳐 나머지 구역(remainder section)을 실행


## Critical Section Problem Solution
![[임계 영역]] 
## OS에서의 Critical section 처리
OS에서는 여러가지 race condition이 발생할 수 있음
ex) fork() system call에서 next_available_pid 가져오기
![[Pasted image 20250408170422.png|200]]
### Preemptive
- 커널 모드에서 실행 중인 프로세스도 선점될 수 있음
- 실제 사용하는 방식
### Non-preemptive
- 커널 모드에서 빠져나가거나, 차단되거나, 자발적으로 CPU를 양보할 때까지 실행함
	- 커널 모드에서는 본질적으로 race condition이 없음
## Peterson's Solution
i의 입장에서의 code
![[Pasted image 20250408171209.png|200]]
자기가 준비되었으면, turn을 j에게 넘기고
내가 턴을 넘겼는데, j가 대기하지 않거나 j가 turn을 나에게 넘기면 내 차례

- 두 프로세스를 대상으로 함
- load와 store가 atomic하다고 가정 (interrupt되지 않음)
- 두 프로세스는 두 개의 변수를 공유함
	- int turn
		- 임계 구역에 들어갈 차례인 프로세스
		- turn = 0: P<sub>0</sub>에게 우선순위가 있음
	- bool flag\[2]
		- 해당 프로세스가 임계 구역에 들어갈 준비가 되었는지 나타냄
		- flag\[i] = true: P<sub>i</sub>가 준비 완료됨
- Critical section problem Solution으로 아래 세 조건을 만족함
	- 상호 배제
		- `P[i]`는 `flag[j] == false`거나, `turn == i` 일때만 임계 구역에 진입함
	- Progress
		- critical section 이후 `flag[i]`를 false로 하므로 progress 만족
	- bounded waiting
		- 한 바퀴 돌면 항상 순서는 반대로 넘어가게 되어있음
단, 피터슨의 해법은 현대 컴퓨터 아키텍처에서는 항상 동작한다고 보장할 수 없음
→ 프로세서나 컴파일러가 의존성이 없는 읽기/쓰기 연산의 순서를 바꿀 수 있음
![[Pasted image 20250408171904.png|300]]
위 순서처럼 뒤바뀔 시 critical section에 둘 다 들어갈 가능성이 생긴다.
또한, turn 값도 atomic하지 않기 때문에, 두 프로세스가 다른 값으로 인식할 수도 있다.
## Critical Section with Interrupt Disable/Enable
간단한 해법:
- 진입 구역에서 인터럽트 비활성화, 종료 구역에서 인터럽트 활성화
- 이렇게 하면 context switching이 일어나지 못한다.
	- 하지만 이 방식은 너무 강력하며, 사용에 제약이 많음
![[Pasted image 20250408172234.png|200]]
## Synchronization Hardware
많은 현대 시스템은 임계 구역 코드를 구현하기 위한 하드웨어 지원을 제공함
- Memory Barriers
	- memory barrier 전과 후에 순서가 바뀌지 못하도록 하는 것
- Hardware Instruction
- Atomic Variables
	- 언어 차원에서 Synchronization Hardware을 사용하여 Atomic을 지원함
## Hardware Instruction: test_and_set (TAS)
현대 컴퓨터는 특별한 원자적 하드웨어 명령어를 제공함
- Atomic = non-interruptible
- 메모리 값을 검사하고 설정하거나(test_and_set) 두 메모리 값의 내용을 교환(swap)하는 방식
```c
boolean test_and_set (boolean *target) {
	boolean rv = *target;
	*target = TRUE;
	return rv;
}
```
- 이 함수는 원자적으로 실행됨
- 인자로 받은 값의 원래 값을 반환하고, 해당 값을 TRUE로 설정함

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
- 하지만, 계속 한 프로세스만 들어갈 수 있어 Bounded Waiting을 보장하지 못함

## Solution with Bounded Waiting
```c
do {
	waiting[i] = true;
	key = 1;
	while (waiting[i] && key == 1) // 내가 기다리고 있는지, key가 있는지 확인
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
