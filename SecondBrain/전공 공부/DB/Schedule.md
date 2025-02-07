## schedule
![[Pasted image 20250207145259.png|200]]
여러 transaction들이 동시에 실행될 때 각 transaction에 속한 operation들의 실행 순서
각 transaction 내의 operation들의 순서는 바뀌지 않는다. (r(K) -> w(K) -> r(H) -> w(H) -> c)
### Serial schedule
transaction들이 겹치지 않고 한 번에 하나씩 실행되는 schedule (sched.1, sched.2)
한 번에 하나의 transaction만 실행되기 때문에 좋은 성능을 낼 수 없고 현실적으로 사용할 수 없는 방식이다.
### Nonserial schedule
transaction들이 겹쳐서 실행되는 schedule
`장점`
- transaction들이 겹쳐서 실행되기 때문에 동시성이 높아져서 같은 시간 동안 더 많은 transaction들을 처리할 수 있다.
`단점`
- transaction들이 어떤 형태로 겹쳐서 실행되는지에 따라 이상한 결과가 나올 수 있다.
=> 따라서 RDBMS는 여러 transaction을 동시에 실행해도 schedule이 conflict serializable하도록 보장하는 프로토콜을 적용한다.
### unrecoverable schedule
schedule 내에서 commit된 transaction이 rollback된 transaction이 write 했었던 데이터를 읽은 경우
=> 이는 rollback을 해도 이전 상태로 회복 불가능할 수 있기 때문에 이런 schedule은 DBMS가 허용하면 안된다.
![[Pasted image 20250207153653.png|300]]
=> Transaction1이 롤백을 진행해서 Transaction2는 유효하지 않은 데이터에 20만원을 더하고 commit 해버린 상황.
#### 그렇다면 어떤 schedule이 recoverable한가?

현재 1번 transaction이 2번 transaction에 의존하고 있다.
이 상태에서는 2번 transaction이 먼저 commit을 하고, 1번 transaction이 커밋해야 한다.
만약 2번 transaction이 rollback해야 한다면, 1번 transaction도 같이 abort를 하여 rollback할 수 있도록 해야한다.

![[Conflict]]