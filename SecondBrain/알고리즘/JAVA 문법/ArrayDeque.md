배열 기반의 앞쪽 추가/샂게가 상수 시간으로 빠른 자료구조
원형 버퍼 구조를 사용한다.
따라서 시작점이 고정되어 있는 ArrayList와 다르게 ArrayDeque는 시작점과 끝점이 움직이는 구조이다.

LinkedList는 노드를 새로 만들 때마다 힙 메모리의 임의의 위치에 할당하기 때문에 캐시지역성이 좋지 않지만, ArrayDeque는 캐시 지역성이 높은 특징이 있다.

또한, 대부분의 경우에서 LinkedList보다 메모리를 적게 사용한다. LinkedList는 데이터를 저장하기 위해 Wrapper class인 Node 객체를 생성하는데, 이 비용이 오버헤드가 크다.
데이터 하나를 저장하려면 `Node` 객체가 하나 필요합니다.
1. Object Header: 12 byte (자바 객체 기본 메타데이터)
2. prev 참조: 4 byte (이전 노드 주소)
3. next 참조: 4 byte (다음 노드 주소)
4. item 참조: 4 byte (실제 데이터 주소)
5. Padding: 0~4 byte (8의 배수로 맞추기 위함)
> 총합: 데이터 1개당 약 24 byte 소모 (데이터 자체 제외하고 순수 관리 비용만)
## 단점
- Null 저장 불가
	- ArrayDeque 특성상 원형 버퍼 구조를 사용하기 때문에 데이터가 null인지, 큐가 비어서 null인지 구분하지 못하기 때문
- 중간 삽입/삭제 시 LinkedList보다 불리
- ArrayDoubling 비용 존재

## 결론
Null 저장 / 중간 삽입/삭제가 필요하지 않다면 LinkedList보다 ArrayDequeue가 성능이 더 좋다.
Queue 사용시에도 ArrayDequeue를 활용하자.