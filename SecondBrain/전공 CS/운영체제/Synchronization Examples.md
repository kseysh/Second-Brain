## 동기화의 고전적인 문제들
•	새로운 동기화 기법을 테스트하는 데 사용되는 고전적인 문제들
	•	유한 버퍼 문제 (Bounded-Buffer Problem)
	•	읽기-쓰기 문제 (Readers and Writers Problem)
	•	식사하는 철학자 문제 (Dining-Philosophers Problem)
## Bounded-Buffer Problem
```cpp
/* Producer */
while (true) {
	/* produce an item in next produced */
	while (counter == BUFFER_SIZE) ;
	/* do nothing */
	buffer[in] = next_produced;
	in = (in + 1) % BUFFER_SIZE;
	counter++;
}
/* Consumer */
while (true) {
	while (counter == 0);
	/* do nothing */
	next_consumed = buffer[out];
	out = (out + 1) % BUFFER_SIZE;
	counter--;
	/* consume the item in next consumed */
}
```
•	n개의 버퍼가 있으며, 각 버퍼는 하나의 항목만 저장 가능
	•	버퍼 크기 = n
•	세마포어 mutex는 1로 초기화됨 (mutex -> buffer 접근에 대한 mutual exclusion 보장하는 semaphore)
•	세마포어 full은 0으로 초기화됨  (full -> n개 중에 몇 개가 사용되고 있는지)
•	세마포어 empty는 n으로 초기화됨 (empty -> n개 중에 비어있는 buffer의 수)
### 생산자 프로세스 구조
```cpp
do {
  …
  /* next_produced에 항목을 생성 */
  …
  wait(empty); // 접근 가능한 빈 semaphore가 있는지 check
  wait(mutex); // buffer는 공유객체이므로 잠그고 들어감
  …
  /* next_produced를 버퍼에 추가 */
  …
  signal(mutex); // 
  signal(full); // 
} while (true);
```
### 소비자 프로세스 구조
```cpp
do {
  wait(full); // buffer에 유효한 data가 채워졌는지 check
  wait(mutex); // buffer는 공유객체이므로 잠그고 들어감
  …
  /* 버퍼에서 항목을 제거하여 next_consumed에 저장 */
  …
  signal(mutex); // 
  signal(empty); // 
  …
  /* next_consumed에 있는 항목을 소비 */
  …
} while (true);
```
## Readers-Writers Problem
•	하나의 데이터 집합을 여러 동시 실행 프로세스가 공유함
	•	Reader(읽기 전용): 데이터를 읽기만 하고 갱신하지 않음
	•	Writer(쓰기 가능): 데이터를 읽고 쓸 수 있음
•	문제: 여러 Reader가 동시에 읽을 수 있도록 허용하되, Writer는 단독으로 접근해야 함 (Write 중 다른 Reader가 읽어도 안 됨)
•	Reader와 Writer 우선순위에 따라 여러 변형 존재
•	공유 데이터
	•	Data Set (Reader/Writer가 읽고 쓰려고 하는 데이터)
	•	세마포어 rw_mutex = 1 (하나의 writer만 접근하도록 하는 mutex)
	•	세마포어 mutex = 1 (read_count 접근 통제용 mutex)
	•	정수 read_count = 0
### Writer 프로세스 구조
```cpp
do {
  wait(rw_mutex);
  /* 쓰기 작업 수행 */
  signal(rw_mutex);
} while (true);
```
### Reader 프로세스 구조
```cpp
do {
  wait(mutex);
  read_count++;
  if (read_count == 1)
    wait(rw_mutex); // 최초로 진입하는 reader만 이 과정 수행
  signal(mutex); // 들어왔으니 read_count에 대한 접근 권한을 해제해줌

  /* 읽기 작업 수행 */

  wait(mutex);
  read_count--;
  if (read_count == 0)
    signal(rw_mutex); // 남은 Reader가 아무도 없으면 Writer가 들어올 수 있도록 signal(rw_mutex)를 날려줌
  signal(mutex);
} while (true);
```
## Readers-Writers Problem Variations
•	First variation (Reader 우선): Writer가 접근 권한을 얻지 않은 이상 Reader는 기다리지 않음
•	Second variation (Writer 우선): Writer가 대기 중이면 가능한 빨리 쓰기 수행
•	두 방식 모두 기아(starvation) 문제가 발생할 수 있으며, 이를 해결하기 위한 다양한 방식 존재
•	일부 시스템에서는 커널이 reader-writer lock을 제공해 문제를 해결함
## Readers-Writers Problem - Writer preference

semaphore rw_mutex = 1;
semaphore mutex = 1;
int read_count = 0;
semaphore queue = 1;  (Writer가 수행 중일 때 새 Reader를 차단)
=> queue는 Reader가 Writer보다 먼저 들어가는 것을 방지하기 위한 진입용 차단 도구이므로 진입만 보호하면 된다.
###### Writer
```cpp
do {
	wait (queue);
	wait (rw_mutex);
	
	/* writing is performed */
	
	signal(rw_mutex);
	signal(queue);
} while (true);
```
###### Reader
```cpp
do{
	wait(queue);
	// 2. signal(queue) 이건 유효함
	wait(mutex);
	// 1. 여기 wait(queue)하면 데드락 발생
	
	read_count++;
	if (read_count == 1)
		wait(rw_mutex);
		
	signal(mutex);
	signal(queue);

	/ * reading is performed */
	
	wait(mutex);
		
	read_count--;
	if (read_count == 0)
		signal(rw_mutex);
		
	signal(mutex);
	
} while (true);
```
wait(queue), signal(queue)의 위치에 대해서 잘 고민해보자

1번에서 deadlock이 발생하는 상황: 1번 Reader가 reading 중이고, 2번 Reader가 wait(mutex) 통과 Writer는 wait(rw_mutex) 대기 중이라면, 
1번 Reader는 rw_mutex를 가지고 mutex를 기다리고
2번 Reader는 mutex를 가지고 queue를 기다리고
Writer는 queue를 가지고 rw_mutex를 가지는 데드락 상황이 발생함

## 식사하는 철학자 문제
![[Pasted image 20250508172347.png|100]]
•	철학자들은 생각과 식사를 번갈아 함
•	이웃 철학자와 상호작용하지 않으며, 식사를 위해 가끔씩 젓가락 두 개를 하나씩 집어 들려고 시도
•	식사를 위해 두 젓가락이 모두 필요하며, 식사 후에는 두 젓가락 모두 내려놓음
-	5명의 철학자 예시
	- 공유 데이터
	-	밥그릇 (데이터 집합)
	-	세마포어 `chopstick[5] = 1`

해법 1: 세마포어 사용
	`semaphore chopstick[5];`
```cpp
do {
  wait(chopstick[i]);
  wait(chopstick[(i+1) % 5]);
  // 식사
  signal(chopstick[i]);
  signal(chopstick[(i+1) % 5]);
  // 생각
} while (true);
```
•	문제: 데드락 발생 가능

## 데드락을 피하는 방법
1.	동시에 식탁에 앉을 수 있는 철학자를 최대 4명으로 제한
2.	두 젓가락이 모두 있을 때만 집을 수 있게 함 (임계 구역 사용)
3.	비대칭 해법 사용:
	•	홀수 번호 철학자는 왼쪽 먼저, 그다음 오른쪽
	•	짝수 번호 철학자는 오른쪽 먼저, 그다음 왼쪽
### asymmetric solution
```cpp
do{
if (i%2==1){
	wait (chopstick[i]);
	wait (chopstick[i+1]%5);
}else{
	wait (chopstick[i+1]%5);
	wait (chopstick[i]);
}
// eat

signal (chopstick[i]);
signal (chopstick[i+1]%5);

// think

}while(true)

```
## Solution 2: Monitor solution
•	`enum {THINKING, HUNGRY, EATING} state[5];`
	•	철학자 i는 두 이웃이 식사 중이 아닐 때만 `state[i] = EATING` 가능
•	`condition self[5];`
	•	철학자 i는 젓가락을 얻지 못하면 대기

```cpp
monitor DiningPhilosophers {
	enum {THINKING, HUNGRY, EATING} state[5];
	condition self[5];
	
	initialization_code() {
		for (int i=0; i<5; i++)
			state[i] = THINKING;
	}

	void test(int i) {
		if ((state[(i+4)%5] != EATING && // 왼쪽 이웃이 식사 중이 아니고
		(state[i] == HUNGRY) && // 내가 배고플 때
		(state[(i+1)%5] != EATING)) { // 오른쪽 이웃이 식사 중이 아니고
			state[i] = EATING; // 위 조건을 충족해야 먹을 수 있음
			self[i].signal(); // 철학자 i에게 식사해도 된다고 알림
		}
	}

	void pickup(int i) {
		state[i] = HUNGRY;
		test(i);
		if (state[i] != EATING) self[i].wait; // state[i] == HUNGRY 라면 (철학자 i가 식사할 수 없으면 대기)
	}
	
	void putdown(int i) {
		state[i] = THINKING;
		test((i+4)%5);
		test((i+1)%5);
	}
}
```

해법 구조
각 철학자는 다음 순서로 pickup()과 putdown() 연산을 호출한다
```cpp
DiningPhilosophers.pickup(i);
/* 식사 */
DiningPhilosophers.putdown(i);
```
•	데드락 없음, 그러나 기아 발생 가능 (오른쪽 왼쪽 사람만 계속 먹을 수도 있기 때문)
## 동기화 예시 - Windows
•	단일 프로세서 시스템: 인터럽트 마스크로 전역 자원 보호
•	다중 프로세서 시스템: 스핀락(spinlock) 사용
	•	스핀락 중인 스레드(spinlock을 통과해서 critical section에 있는 스레드)는 선점되지 않음
•	사용자 공간에 디스패처 객체 제공 - 뮤텍스, 세마포어, 이벤트, 타이머 역할 가능
	•	이벤트(Event): 조건 변수와 유사하게 작동
	•	타이머: 시간이 만료되면 하나 이상의 스레드에 알림 전달
	•	디스패처 객체는 신호 상태 (사용 가능) 또는 비신호 상태 (스레드 대기)
## 동기화 예시 - Linux
•	커널 2.6 이전: 짧은 임계 구역 구현을 위해 인터럽트 비활성화
•	커널 2.6 이후: 완전 선점 가능

•	Linux는 다음을 제공:
	•	세마포어
	•	원자적 정수 연산
	•	atomic_t 타입과 원자 연산
		`atomic_t counter;` `int value;`
	![[Pasted image 20250508171624.png|300]]
•	스핀락(Spinlock) => api 형태로 제공