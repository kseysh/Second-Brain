## java.util.concurrent.locks 패키지를 이용한 동기화
- `ReetrantLock` 
	- 재진입이 가능한 lock, 가장 일반적인 배타 lock
- `ReetrantLockReadWriteLock` 
	- 읽기에는 공유적이고, 쓰기에는 배타적인 lock
- `StampedLock`
	- ReetrantLockReadWriteLock에 낙관적인 lock의 기능을 추가

![[JAVA/낙관적 락|낙관적 락]] [[DB/낙관적 락|낙관적 락]]