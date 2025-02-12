## Write Lock (exclusive Lock)
write할 때 사용한다.
다른 트랜잭션의 write와 read를 모두 막는다.
## Read Lock (Share Lock)
Read할 때 사용한다.
다른 트랜잭션의 write만을 막는다. (다른 tx의 read는 가능하다.)

## Lock을 사용했을 때 Serializable하지 않은 예제 (2PL protocol이 필요한 이유)
### serial schedule
![[Pasted image 20250212161211.png|400]]
![[Pasted image 20250212161237.png|400]]
### nonserializable한 nonserinal schedule
![[Pasted image 20250212161400.png|400]]

#### 해결 방법
Nonserializable한 문제를 해결하기 위해서 현재 
read_lock(y) -> unlock(y) -> write_lock(x)를
read_lock(y) ->  write_lock(x) -> unlock(y)의 순서로 락 획득 후 락 반환의 순서를 가지면 serializable한 순서를 지닐 수 있다.

## 2PL protocol (two-phase locking)
위에 설명한 것 처럼 tx에서 모든 locking operation이 최초의 unlock operation보다 먼저 수행되도록 하는 것을 2PL protocol이라 한다.

### Expanding phase
2PL protocol에서 lock을 취득하기만 하고 반환하지 않는 phase
### Shrinking phase
2PL protocol에서 lock을 반환만 하고 취득하지 않는 phase

## 2PL과 DeadLock
![[Pasted image 20250212162333.png|400]]
2PL 방식의 특성상 Lock을 반환하는 Shrink phase 전에 Expanding phase에서 Lock을 모두 획득하기 전까지 반환하지 않는다.
따라서 이 때문에 다들 Lock을 획득하려고만 해서 DeadLock이 발생할 수 있다.
### 이를 해결하기 위한 방법
#### conservative 2PL
모든 Lock을 확보해야 tx를 시작한다. (Lock을 거는 것이 아니라 Lock을 확보할 수 있는지 확인 -> 확보 가능하다면 Lock을 건다)
어떤 트랜잭션도 일부 Lock을 가진 채 다른 Lock을 기다리지 않는다.
하지만 모든 Lock을 취득하는 것이 시간이 오래 걸릴 수 있어 실용적이지 않다.
#### strict 2PL
![[Pasted image 20250212170306.png|100]]
strict schedule을 보장하는 2PL
recoverablility 보장
write-lock을 commit/rollback될 때 반환 (write lock인 y와 z를 commit 후 unlock함)
#### strong strict 2PL
strict schedule을 보장하는 2PL
recoverablility 보장
read-lock, write-lock을 commit/rollback될 때 반환 (write lock인 y와 z외에 read lock인 x도 commit 후 unlock함)
S2PL보다 구현이 쉽지만, 락을 오래 쥐게 된다.
