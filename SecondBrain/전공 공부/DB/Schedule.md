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

## Conflict
두 개의 operation이 세 가지 조건을 모두 만족하면 conflict라 한다
1. 서로 다른 transaction 소속
2. 같은 데이터에 접근
3. 최소 하나는 write operation

conflict operation은 순서가 바뀌면 결과도 바뀐다.

## Conflict equivalent
두 개의 schedule이 두 가지 조건을 모두 만족하면 conflict equivalent라 한다.
1. 두 schedule은 같은 transaction들을 가진다
2. 어떤 conflicting operation의 순서도 양쪽 schedule 모두 동일하다.

아래에서 모든 conflicting operation의 순서가 양 쪽이 동일하므로 schedule 2와 schedule 3은 Conflict equivalent하다.
또한 sched.3은 serial schedule인 sched.2와 conflict equivalent이므로 `conflict serializable`이라고 한다
r1(K) -> w2(H)
w2(H) -> r1(K)
이렇게 두 순서는 스케쥴에 따라 다르긴 하지만, 같은 데이터에 접근하지 않으므로 Conflict가 아니다. Conflict를 만족하는 operation의 순서가 동일해야 하는 것이다.
![[Pasted image 20250207151347.png|150]]![[Pasted image 20250207151459.png|150]]