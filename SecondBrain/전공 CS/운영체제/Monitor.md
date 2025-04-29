## 왜 모니터(Monitor)를 사용하는가?
•	동기화 장치로서 세마포어는 <u>구조화되지 않은 방식</u>이다
	•	동기화 버그, 즉 경쟁 조건(race condition)에 취약하다
	•	너무 저수준이다
•	프로세스 동기화를 위해 편리하고 효과적인 메커니즘을 제공하는 <u>고수준 추상화</u>가 필요하다
	•	고수준의 상호배제 의미 제공
		•	<u>한 시점에 하나의 스레드만 모니터 내부에서 활성 상태</u>
	•	잠금(lock)은 숨겨져 있으며
		•	임계 구역에 들어가거나 나갈 때 자동으로 잠금/잠금 해제됨
## 모니터
•	동기화를 위해 편리하고 효과적인 메커니즘을 제공하는 고수준 추상화
•	<u>추상 자료형(ADT)</u>으로, <u>내부 변수는 해당 프로시저 내의 코드만 접근 가능</u>
•	한 번에 하나의 프로세스만 모니터 내에서 활성일 수 있음
•	그러나 일부 동기화 방식을 모델링하기에는 부족한 점이 있음

모니터는 다음을 캡슐화함
•	공유 데이터 구조
•	공유 데이터를 조작하는 프로시저들
•	해당 프로시저를 호출하는 동시 스레드 간의 동기화
![[Pasted image 20250429165853.png|250]]
•	모니터의 개략적인 구조
![[Pasted image 20250429165914.png|200]]
•	모니터 내 Condition variables (waiting queues) => 순서 동기화를 위한 장치
	•	condition x, y; (x라는 이름의 큐를 만든다고 생각한다 => 큐는 waiting queue 같은거 구현 시 사용)
•	조건 변수에 대해 허용되는 두 가지 연산:
	•	x.wait() : 이 연산을 호출한 프로세스는 x.signal()이 호출될 때까지 중단됨
	•	x.signal() : x.wait()을 호출해 대기 중인 프로세스 중 하나를 재개
		•	대기 중인 프로세스가 없다면 아무 영향 없음 (기존 semaphore 연산과는 조금 차이가 있음)

•	조건 변수를 갖는 모니터
![[Pasted image 20250429170028.png|300]]
일반적으로, x.wait(time)에서의 time => timeout 값으로, 기다리는 maximum 시간을 정해줌
![[Pasted image 20250429170044.png|200]]
acquire(time) => time만큼 자원을 할당받아 사용
release() => 자원 해제
busy => 자원이 사용되고 있는지
여기서 x.wait(time)에서의 time => 우선 순위를 나타내기 위해 사용 (낮은 time 먼저 깨우기로 약속함)
### Condition variable choices
•	프로세스 P가 x.signal()을 호출하고, 프로세스 Q가 x.wait()으로 중단되어 있는 경우 다음에 어떤 일이 일어나야 하는가?
	•	Q와 P는 동시에 실행될 수 없음. Q가 재개된다면 P는 대기해야 함
•	가능한 선택지:
	•	Signal and wait : P는 Q가 모니터를 나가거나 다른 조건을 기다릴 때까지 대기
	•	Signal and continue : Q는 P가 모니터를 나가거나 다른 조건을 기다릴 때까지 대기
	•	두 방식 모두 장단점이 있으며 언어 구현자가 선택할 수 있음
		•	Concurrent Pascal 언어에서 구현된 모니터는 타협안 사용:
			•	P가 signal을 호출하면 즉시 모니터를 나가고, Q는 재개됨

## Monitor Implementation (signal and wait 방식)
각 모니터 내 변수들
```c
semaphore monitor_lock; // 초기값 1
semaphore sig_lock;     // 초기값 0
int sig_lock_count = 0;
```
monitor 바깥에 queue가 2개 있음
- entry queue
- signaler queue
이 두 큐를 관리하기 위해 semaphore를 이용 (monitor_lock, sig_lock)

•	각 프로시저 F는 다음과 같이 대체됨:
```c
wait(monitor_lock);
// F 본문 ...
if (sig_lock_count > 0)
    signal(sig_lock); // signaler queue에 signal을 날림
else
    signal(monitor_lock); // entry queue에 signal을 날림
```

모니터 구현 시 조건 변수에 필요한 변수들
```c
int x_count;        // 초기값 0
semaphore x_sem;    // 초기값 0
```