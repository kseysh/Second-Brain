## schedule
![[Pasted image 20250207145259.png|200]]
여러 transaction들이 동시에 실행될 때 각 transaction에 속한 operation들의 실행 순서
각 transaction 내의 operation들의 순서는 바뀌지 않는다. (r(K) -> w(K) -> r(H) -> w(H) -> c)

### Serial schedule
transaction들이 겹치지 않고 한 번에 하나씩 실행되는 schedule (sched.1, sched.2)
한 번에 하나의 transaction만 실행되기 때문에 좋은 성능을 낼 수 없고 현실적으로 사용할 수 없는 방식이다.
### Nonserial schedule
transaction들이 겹쳐서 실행되는 schedule