## java.util.concurrent.locks 패키지를 이용한 동기화
- `ReetrantLock` 
	- 재진입이 가능한 lock, 가장 일반적인 배타 lock
- `ReetrantLockReadWriteLock` 
	- 읽기에는 공유적이고, 쓰기에는 배타적인 lock
- `StampedLock`
	- ReetrantLockReadWriteLock에 낙관적인 lock의 기능을 추가
## 낙관적 락
공유 데이터에 대한 충돌 가능성을 낮게 가정하고 작업을 수행하는 방식으로, 자원의 잠금을 최소화하여 성능을 높이는데 초점을 둔다.

### 자
```java
int getBalance() {
    long stamp = lock.tryOptimisticRead(); // 낙관적 읽기 lock을 건다.

    int curBalance = this.balance; // 공유 데이터인 balance를 읽어온다.

    if (lock.validate(stamp)) { // 쓰기 lock에 의해 낙관적 읽기 lock이 풀렸는지 확인
        stamp = lock.readLock(); // lock이 풀렸으면, 읽기 lock을 얻으려고 시도한다.

        try {
            curBalance = this.balance; // 공유 데이터를 다시 읽어온다.
        } finally {
            lock.unlockRead(stamp); // 읽기 lock을 푼다.
        }
    }

    return curBalance; // 낙관적 읽기 lock이 풀리지 않았으면 곧바로 읽어온 값을 반환
}
```