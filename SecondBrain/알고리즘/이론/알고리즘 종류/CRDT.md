분산 시스템 환경에서 여러 사용자가 동시에 데이터를 변경하더라도, 별도의 중앙 서버의 복잡한 중재 없이 수학적으로 항상 동일한 최종 상태에 도달함을 보장하는 데이터 구조 (= Conflict-free Replicated Data Type)

## CRDT가 만족해야 하는 수학적 특성
- 교환법칙
- 결합법칙
- 멱등성
=> 이를 통해 강한 최종적 일관성을 보장할 수 있다.
## 주요 구현 방식

### State-based (CvRDT: Convergent Replicated Data Types)
- Local의 상태 전체를 다른 노드로 보낸다.
- 받은 상태와 내 상태를 합친다 
	- 상태를 합치더라도, 교환법칙을 만족해야 한다.
- 전송 비용이 크다
### Operation-based (CmRDT: Commutative Replicated Data Types)
- 변경된 연산만 보낸다.
- 연산은 교환법칙을 만족해야 한다.
- 연산이 유실되면 문제가 발생할 수 있다.