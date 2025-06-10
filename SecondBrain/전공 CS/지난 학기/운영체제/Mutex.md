## Mutex
![[Pasted image 20250104001334.png|450]]
- 공유 자원에 대해 lock이 걸려있다면 현재 스레드를 큐에 넣는다. (sleep으로 인해 context switching이 발생)
- lock을 반환할 때 대기 큐에 하나라도 있다면 그 중 하나를 깨운다. 
	- 이로 인해 CPU 낭비를 최소화할 수 있다.
- guard => value라는 값도 공유 자원이므로 value도 critical section안에서 보호받으면서 값을 바꿔줘야 하기 때문에 사용한다.
	- 이는 [[spinlock]]으로 구현된다.