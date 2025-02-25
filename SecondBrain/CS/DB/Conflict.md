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