## java.util.concurrent.locks 패키지를 이용한 동기화
- `ReetrantLock` 
	- 재진입이 가능한 lock, 가장 일반적인 배타 lock
- `ReetrantLockReadWriteLock` 
	- 읽기에는 공유적이고, 쓰기에는 배타적인 lock
- `StampedLock`
	- ReetrantLockReadWriteLock에 낙관적인 lock의 기능을 추가
## ReetrantLock
synchronized 대신 lock()과 unlock()을 사용
![[Pasted image 20241227211802.png|450]]
### ReetrantLock과 Condition으로 스레드를 구분해서 `wait()` & `notify()`
ReetrantLock을 이용해 동기화를 하면 기존 synchronized의 단점이었던 스레드를 구분해서 동기화할 수 없다는 단점을 해결할 수 있다.
![[Pasted image 20241227212131.png|350]]
![[Pasted image 20241227212120.png|350]]

![[JAVA/낙관적 락|낙관적 락]] [[DB/낙관적 락|낙관적 락]]