# Lock을 사용한다.
![[Pasted image 20250103232831.png|250]]
## spinlock
락을 가질 수 있을 때까지 계속 반복해서 시도
![[Pasted image 20250103235737.png|400]]
여기서 TestAndSet는 CPU atomic 명령어이다.
 - 실행 중간에 간섭받거나 중단되지 않는다
 - 같은 메모리 영역에 대해 동시에 실행되지 않는다.
### 특징
- 락이 해제될 때까지 계속 루프를 돌기 때문에 CPU 자원을 계속 소비한다.
### Mutex보다의 장점
- 멀티 코어 환경이고, 임계 영역에서의 작업이 컨텍스트 스위칭보다 더 빨리 끝난다면 스핀락이 뮤텍스보다 더 이점이 있다.
	- Thread 1이 Thread 2를 spinlock하면서 기다리는 경우에만 이점이 있으므로 멀티 코어 환경이어야 한다.
		- 싱글코어라면 어차피 context switching이 발생해야 하므로 뮤텍스보다 context switching을 덜 한다는 단점이 사라지게 된다.
## Mutex
![[Pasted image 20250104001334.png|450]]
- 공유 자원에 대해 lock이 걸려있다면 현재 스레드를 큐에 넣는다. (sleep으로 인해 context switching이 발생)
- lock을 반환할 때 대기 큐에 하나라도 있다면 그 중 하나를 깨운다. 
	- 이로 인해 CPU 낭비를 최소화할 수 있다.
- guard => value라는 값도 공유 자원이므로 value도 critical section안에서 보호받으면서 값을 바꿔줘야 하기 때문에 사용한다.
	- 이는 spinlock으로 구현된다.

## semaphore
signal mechanism을 가진 하나 이상의 스레드가 임계 영역에 접근 가능하도록 하는 장치
![[Pasted image 20250104002412.png|450]]
세마포어에서는 임계 영역에 하나 이상의 스레드가 들어올 수 있도록 설정할 수 있다. (value의 값을 조절해서)
### 세마포는 순서를 정해줄 때 사용한다
![[Pasted image 20250104002750.png|450]]
task3는 작업을 실행하기 위해서 반드시 task1의 작업이 필요하게 된다.
이처럼 task1 & task2 -> task3의 순서를 보장할 수 있다.