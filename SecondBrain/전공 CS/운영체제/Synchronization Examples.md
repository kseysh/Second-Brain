동기화의 고전적인 문제들
	•	새로운 동기화 기법을 테스트하는 데 사용되는 고전적인 문제들
	•	유한 버퍼 문제 (Bounded-Buffer Problem)
	•	읽기-쓰기 문제 (Readers and Writers Problem)
	•	식사하는 철학자 문제 (Dining-Philosophers Problem)

⸻

유한 버퍼 문제 (1)
	•	6장에서 다룬 소비자-생산자 문제로 돌아가기
	•	n개의 버퍼가 있으며, 각 버퍼는 하나의 항목만 저장 가능
	•	버퍼 크기 = n
	•	세마포어 mutex는 1로 초기화됨
	•	세마포어 full은 0으로 초기화됨
	•	세마포어 empty는 n으로 초기화됨

생산자 프로세스 구조

do {
  …
  /* next_produced에 항목을 생성 */
  …
  wait(empty);
  wait(mutex);
  …
  /* next_produced를 버퍼에 추가 */
  …
  signal(mutex);
  signal(full);
} while (true);

소비자 프로세스 구조

do {
  wait(full);
  wait(mutex);
  …
  /* 버퍼에서 항목을 제거하여 next_consumed에 저장 */
  …
  signal(mutex);
  signal(empty);
  …
  /* next_consumed에 있는 항목을 소비 */
  …
} while (true);



⸻

읽기-쓰기 문제 (1)
	•	하나의 데이터 집합을 여러 동시 실행 프로세스가 공유함
	•	Reader(읽기 전용): 데이터를 읽기만 하고 갱신하지 않음
	•	Writer(쓰기 가능): 데이터를 읽고 쓸 수 있음
	•	문제: 여러 Reader가 동시에 읽을 수 있도록 허용하되, Writer는 단독으로 접근해야 함
	•	Reader와 Writer 우선순위에 따라 여러 변형 존재
	•	공유 데이터
	•	데이터 집합
	•	세마포어 rw_mutex = 1
	•	세마포어 mutex = 1
	•	정수 read_count = 0

Writer 프로세스 구조

do {
  wait(rw_mutex);
  /* 쓰기 작업 수행 */
  signal(rw_mutex);
} while (true);

Reader 프로세스 구조

do {
  wait(mutex);
  read_count++;
  if (read_count == 1)
    wait(rw_mutex);
  signal(mutex);

  /* 읽기 작업 수행 */

  wait(mutex);
  read_count--;
  if (read_count == 0)
    signal(rw_mutex);
  signal(mutex);
} while (true);



⸻

읽기-쓰기 문제의 변형
	•	첫 번째 변형 (Reader 우선): Writer가 접근 권한을 얻지 않은 이상 Reader는 기다리지 않음
	•	두 번째 변형 (Writer 우선): Writer가 대기 중이면 가능한 빨리 쓰기 수행
	•	두 방식 모두 기아(starvation) 문제가 발생할 수 있으며, 이를 해결하기 위한 다양한 방식 존재
	•	일부 시스템에서는 커널이 reader-writer lock을 제공해 문제를 해결함

Writer 우선 구현 예

semaphore rw_mutex = 1;
semaphore mutex = 1;
int read_count = 0;
semaphore queue = 1; // Writer가 수행 중일 때 새 Reader를 차단

	•	[연습] Writer 코드에 wait(queue)와 signal(queue)를 적절히 삽입

⸻

식사하는 철학자 문제 (1)
	•	철학자들은 생각과 식사를 번갈아 함
	•	이웃 철학자와 상호작용하지 않으며, 식사를 위해 가끔씩 젓가락 두 개를 하나씩 집어 들려고 시도
	•	식사를 위해 두 젓가락이 모두 필요하며, 식사 후에는 두 젓가락 모두 내려놓음
	•	5명의 철학자 예시

공유 데이터
	•	밥그릇 (데이터 집합)
	•	세마포어 chopstick[5] = 1

해법 1: 세마포어 사용

semaphore chopstick[5];
do {
  wait(chopstick[i]);
  wait(chopstick[(i+1) % 5]);
  // 식사
  signal(chopstick[i]);
  signal(chopstick[(i+1) % 5]);
  // 생각
} while (true);

	•	문제: 데드락 발생 가능

데드락을 피하는 방법
	1.	동시에 식탁에 앉을 수 있는 철학자를 최대 4명으로 제한
	2.	두 젓가락이 모두 있을 때만 집을 수 있게 함 (임계 구역 사용)
	3.	비대칭 해법 사용:
	•	홀수 번호 철학자는 왼쪽 먼저, 그다음 오른쪽
	•	짝수 번호 철학자는 오른쪽 먼저, 그다음 왼쪽

⸻

식사하는 철학자 문제 (6): 모니터 해법
	•	enum {THINKING, HUNGRY, EATING} state[5];
	•	철학자 i는 두 이웃이 식사 중이 아닐 때만 state[i] = EATING 가능
	•	condition self[5]; 사용
	•	철학자 i는 젓가락을 얻지 못하면 대기

해법 구조

DiningPhilosophers.pickup(i);
/* 식사 */
DiningPhilosophers.putdown(i);

	•	데드락 없음, 그러나 기아 발생 가능

⸻

동기화 예시 - Windows
	•	단일 프로세서 시스템: 인터럽트 마스크로 전역 자원 보호
	•	다중 프로세서 시스템: 스핀락(spinlock) 사용
	•	스핀락 중인 스레드는 선점되지 않음
	•	사용자 공간에 디스패처 객체 제공
	•	뮤텍스, 세마포어, 이벤트, 타이머 역할 가능
	•	이벤트(Event): 조건 변수와 유사하게 작동
	•	타이머: 시간이 만료되면 하나 이상의 스레드에 알림 전달
	•	디스패처 객체는 신호 상태 (사용 가능) 또는 비신호 상태 (스레드 대기)

⸻

동기화 예시 - Linux
	•	커널 2.6 이전: 짧은 임계 구역 구현을 위해 인터럽트 비활성화
	•	커널 2.6 이후: 완전 선점 가능
	•	Linux는 다음을 제공:
	•	세마포어
	•	원자적 정수 연산
	•	atomic_t 타입과 원자 연산

atomic_t counter; 
int value;

atomic_set(&counter, 5);        // counter = 5
atomic_add(10, &counter);       // counter += 10
atomic_sub(4, &counter);        // counter -= 4
atomic_inc(&counter);           // counter += 1
value = atomic_read(&counter);  // value = 12

	•	스핀락(Spinlock)
