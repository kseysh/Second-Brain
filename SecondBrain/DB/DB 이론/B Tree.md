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
### 삽입
데이터 추가는 항상 leaf노드에 한다.
노드가 넘치면 가운데 key를 기준으로 좌우 key들은 분할하고 가운데 key는 승진한다.