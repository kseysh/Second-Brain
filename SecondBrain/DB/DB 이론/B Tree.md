Binary Tree가 자신의 key값을 기준으로 자식의 key 값 범위를 지정하는 것 처럼, 
B Tree는 자녀 노드의 최대 개수를 늘리기 위해서 한 노드에 key를 하나 이상 저장하고, 
key를 오름차순으로 정렬해 
정렬된 순서에 따라 자녀 노드들의 key값 범위를 결정한다.
=> BST를 일반화한 Tree
![[Pasted image 20250509212757.png|200]]

## B Tree 주요 파라미터
M : 각 노드의 최대 자녀 노드 수
=> 최대 M개의 자녀를 가질 수 있는 B Tree를 M차 B Tree라고 한다.
M-1 : 각 노드의 최대 key 수
⌈M/2⌉ : 각 노드의 최소 자녀 노드 수 (root node, leaf node 제외)
⌈M/2⌉ - 1: 각 노드의 최소 key 수 (root node 제외) 

## B Tree 규칙
internal 노드의 key 수가 x개라면, 자녀 노드의 수는 언제나 x+1개다.
모든 leaf 노드들은 같은 레벨에 있다 (balanced tree)
### 삽입
데이터 추가는 항상 leaf노드에 한다.
노드가 넘치면 가운데 key를 기준으로 좌우 key들은 분할하고 가운데 key는 승진한다.
#### example
![[Pasted image 20250509221058.png|300]]
여기서 70을 넣으면
![[Pasted image 20250509221122.png|300]]
leaf 노드에 70넣기 -> 70 승격 50, 90 분리
### 삭제
- 데이터 삭제도 항상 leaf 노드에서 발생한다. 
- internal 노드에 있는 데이터를 삭제하려면 leaf 노드에 있는 데이터와 위치를 바꾼 후 삭제한다
	- 삭제할 데이터의 predecessor나 successor와 위치를 바꿔준다.
- 삭제 후 최소 key 수(⌈M/2⌉ - 1)보다 적어졌다면 재조정한다.
- 재조정 과정
1. key 수가 여유있는 형제의 지원을 받는다
	1. 왼쪽 형제가 여유가 있으면
		- 동생의 가장 큰 key를 부모 노드의 나와 동생 사이에 둔다
		- 원래 그 자리에 있던 key는 나의 가장 왼쪽에 둔다
	2. 형이 여유가 있으면
		- 형의 가장 작은 key를 부모 노드의 나와 형 사이에 둔다
		- 원래 그 자리에 있던 key는 나의 가장 오른쪽에 둔다.
2. 1번이 불가능하면 부모의 지원을 받고 형제와 합친다.
	1. 동생이 있으면 동생과 나 사이의 key를 부모로부터 받는다
		- 그 key와 나의 key를 차례대로 동생에게 합친다
		- 나의 노드를 삭제한다
	2. 동생이 없으면 형과 나 사이의 key를 부모로부터 받는다.
		- 그 key와 형의 key를 차례대로 나에게 합친다
		- 형의 노드를 삭제한다.
3. 부모가 지원한 후 부모에 문제가 있다면 상황에 맞게 대응한다
	1. 부모가 root 노드가 아니라면
		- 그 위치에서부터 다시 1번부터 재조정을 시작한다
	2. 부모가 root 노드고 비어있다면
		- 부모 노드를 삭제한다
		- 직전에 합쳐진 노드가 root 노드가 된다.
#### example 1
![[Pasted image 20250509222436.png|200]]
![[Pasted image 20250509222500.png|200]]
#### example 2
![[Pasted image 20250509222843.png|200]]
![[Pasted image 20250509222859.png|200]]
#### example 3
![[Pasted image 20250509230147.png|200]]
![[Pasted image 20250509230459.png|200]]

## DB Index
### B tree 시간 복잡도
![[Pasted image 20250511164307.png|200]]
### self-balancing tree인 AVL Tree와, RB Tree도 시간 복잡도가 같은데, B Tree를 사용하는 이유는?
DB는 secondary stoarage에 저장되고, DB에서 데이터를 조회할 때 secondary stoarage에 최대한 적게 접근하는 것이 성능 면에서 좋기 때문.

#### storage 접근 횟수 측면
AVL, RB Tree와 다르게 B Tree는 한 노드당 가지고 있는 key의 수가 여러개이기 때문에 storage 접근 횟수가 줄어들 수 있어 B Tree를 사용한다.
![[Pasted image 20250511171956.png|300]]
예를 들어, 101차 B Tree에서 best case일 때, 높이 3으로 1억개의 data를 저장할 수 있다.

#### Block 단위로 읽고 쓰는 DB 특성에 관한 측면
DB는 block 단위로 읽고 쓰기 때문에 연관된 데이터를 모아서 저장하면 더 효율적으로 읽고 쓸 수 있다.
![[Pasted image 20250511172414.png|300]]
### Hash는 DB 저장시 사용하면 안될까?
=> 범위 기반 검색이 어려움