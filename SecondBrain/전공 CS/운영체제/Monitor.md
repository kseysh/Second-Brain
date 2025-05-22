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
•	*한 번에 하나의 프로세스만 모니터 내에서 활성일 수 있음*
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
•	Condition variable에 대해 허용되는 두 가지 연산:
	•	x.wait() : 이 연산을 호출한 프로세스는 x.signal()이 호출될 때까지 중단됨
	•	x.signal() : x.wait()을 호출해 대기 중인 프로세스 중 하나를 재개
		•	대기 중인 프로세스가 없다면 아무 영향 없음 (기존 semaphore 연산과는 조금 차이가 있음)

•	조건 변수를 갖는 모니터
![[Pasted image 20250429170028.png|300]]
일반적으로, x.wait(time)에서의 time => timeout 값으로, 기다리는 maximum 시간을 정해줌
- Entry Queue: 모니터 내부로 들어가려는 스레드들이 대기하는 곳 (monitor lock 자체를 기다리는 큐)
- Condition variable Waiting Queue: wait()을 호출한 스레드가 특정 조건이 만족될 때까지 대기하는 큐
	- wait()을 하면 모니터 락을 반환하고 조건 변수 대기 큐에 들어간다.
- signal lock queue: signal 호출 시 모니터 락은 현재 실행 중인 스레드가 가지고 있으므로 깨운 스레드는 즉시 실행할 수 없고 락을 다시 획득할 때까지 sig_lock_queue에서 대기한다.
#### Monitor - Code Example
![[Pasted image 20250429170044.png|200]]
acquire(time) => time만큼 자원을 할당받아 사용
release() => 자원 해제
busy => 자원이 사용되고 있는지
여기서 x.wait(time)에서의 time => 우선 순위를 나타내기 위해 사용 (낮은 time 먼저 깨우기로 약속함)
### Condition variable choices
•	프로세스 P가 x.signal()을 호출하고, 프로세스 Q가 x.wait()으로 중단되어 있는 경우 다음에 어떤 일이 일어나야 하는가?
	•	Q와 P는 동시에 실행될 수 없음. Q가 재개된다면 P는 대기해야 함
•	가능한 선택지:
	•	Signal and wait : P는 sig_lock_queue에서 대기, Q는 실행
	•	Signal and continue : P가 계속 실행, Q는 sig_lock_queue에서 대기 (보통 실제로 이걸 활용)
	•	두 방식 모두 장단점이 있으며 언어 구현자가 선택할 수 있음
		•	Concurrent Pascal 언어에서 구현된 모니터는 타협안 사용:
			•	P는 exit(), Q는 실행
## Monitor Implementation (signal and wait 방식)
각 모니터 내 변수들
```c
semaphore monitor_lock; // 초기값 1
semaphore sig_lock;     // 초기값 0
int sig_lock_count = 0; // sig_lock 날리고 대기하는 프로세스 수
```
monitor 바깥에 queue가 2개 있음
- entry queue
- signaler queue
이 두 큐를 관리하기 위해 semaphore를 이용 (monitor_lock, sig_lock)

•	각 프로시저 F는 다음과 같이 대체됨:
```c
wait(monitor_lock);

// body F ...

if (sig_lock_count > 0)
    signal(sig_lock); // signaler queue에 signal을 날림
else
    signal(monitor_lock); // entry queue에 signal을 날림
```

모니터 구현 시 조건 변수에 필요한 변수들
```c
int x_count;        // 초기값 0 , x에서 기다리는 프로세스 수
semaphore x_sem;    // 초기값 0 , x에 대한 semaphore
```

```cpp
/* x.wait */
x_count++; // 현재 스레드가 x를 기다리므로 대기자 수 증가
if (sig_lock_count > 0) // 이미 누군가 x.signal()을 호출한 상태라면,
	signal(sig_lock); // signal을 기다리던 스레드 하나를 깨움
else
	signal(monitor_lock); // 그렇지 않으면 monitor_lock을 넘겨줌
wait(x_sem); // x.signal()이 호출될 때까지 대기
x_count--; // 일어나서 다 기다렸으니 나갈거라서 x_count를 감소

/* x.signal */
if (x_count > 0) { // 조건 변수 x를 기다리는 스레드가 있다면
	sig_lock_count++; // signal을 기다리는 스레드 수 증가
	signal(x_sem); // 기다리던 스레드 하나를 깨움
	wait(sig_lock); // signal한 스레드는 sig_lock에서 잠시 대기 (깨운 스레드가 모니터를 먼저 갖고 실행을 계속하게 하기 위함)
	sig_lock_count--; // signal 대기 스레드 수 감소
}
```

## Signal and Continue
```cpp
/* x.wait */
add current_thread to x.queue; // x_counter++
signal(monitor_lock);
block current_thread; // wait(x_sem)

acquire monitor_lock // wait(monitor_lock)

/* x.signal */
if(!x.queue.empty()){ // x.count > 0
	move one thread from x.queue to ready queue; // signal(x_sem)
}
```

signal-and-wait 방법에 해당함
프로세스 Q가 x.wait()을 호출하고, 프로세스 P가 x.signal()을 통해 Q를 깨워주는 과정을 보면, signal(x_sem)을 이용해 wait(x_sem)을 통해 잠자고 있던 Q를 깨우고 P는 곧바로 wait(sig_lock)을 이용해 잠자는 것을 볼 수 있다.
이처럼 signal을 보내고 지속하는 것이 아닌 곧바로 wait을 실행하기 때문에 signal-and-wait