## Minimum Spanning Tree
- 스패닝 트리는 연결된 undirected graph G=(V,E)에 대해 G의 모든 정점을 포함하는 subgraph이다.
- weighted graph G=(V,E,W)에서 subgraph의 가중치는 해당 subgraph에 포함된 간선들의 가중치의 합이다.
- 가중치 그래프의 최소 신장 트리는 가중치의 합이 최소인 스패닝 트리이다.
![[Pasted image 20250512151904.png|400]]
## Prim 알고리즘
프림 알고리즘 (Prim’s Algorithm)
•	임의의 시작 정점을 선택한다 (루트).
•	현재까지 구성된 트리에서 뻗어나가며, 매 반복마다 간선을 하나 선택한다.
	•	트리에 연결할 수 있는 모든 간선 중 가중치가 가장 작은 간선을 트리에 추가한다.
	•	간선에 연결된 정점을 트리에 추가한다.
•	알고리즘 수행 중, 정점들은 세 가지 서로 겹치지 않는 범주로 나뉜다:
	•	트리 정점(Tree vertices): 지금까지 구성된 트리에 포함된 정점들,
	•	경계 정점(Fringe vertices): 트리에 속하지는 않지만 트리 내 어떤 정점과 인접한 정점들,
	•	미탐색 정점(Unseen vertices): 그 외의 모든 정점들.
### The Algorithm in Action, Example
![[Pasted image 20250512152240.png|400]]
### Prim’s Algorithm: Outline
![[Pasted image 20250512152304.png|400]]
우리의 prim 알고리즘에서는 정점과 가중치의 쌍을 우선순위 큐에 저장한다. ()
### 시간 복잡도 분석
![[Pasted image 20250530173638.png|500]]
프림 알고리즘은 우선 순위 큐에서 가장 작은 정점을 찾고, 삭제하는 과정을 n번 반복한다.
또한 새로운 후보 정점들을 n번 삽입하므로 삽입과정도 n번 반복한다.
후보 정점이 이미 큐에 있고, 기존보다 작은 비용으로 연결된다면 key값을 감소시켜야 하는데, 이 과정은 최대 m번 일어날 수 있다.
이 때 가장 작은 정점을 찾는 연산이 (  )이고, 삭제하는 연산이 (  )이고, 삽입 연산이 (  )이고, key 값을 감소시키는 연산이 (  )이므로 총 연산은 (  )이다.

프림 알고리즘의 시간 복잡도 T(n, m) = O(n x (T(getMin) + T(deleteMin) + T(insert)) + m x T(decreaseKey))이므로

unsorted array 사용 시: 
최솟 값 가져오는 연산: O(n) (최악의 경우, 모든 )
### Kruskal’s Algorithm: Outline
![[Pasted image 20250512152334.png|400]]
R은 남은 edges, F는 결과인 forest edges
1. 모든 edge의 집합을 오름차순으로 정렬
2. R에서 가장 가중치가 작은 간선을 꺼낸다
3. 간선 vw를 추가했을 때 cycle이 생기지 않는다면 F에 추가
4. n-1개의 간선이 포함되면 종료한다.
## Kruskal 알고리즘을 위한 자료구조
•	알고리즘은 여러 개의 트리(포레스트)를 유지
•	간선이 서로 다른 트리를 연결할 때만 선택
•	서로소 집합(disjoint set)을 유지하기 위한 자료구조 필요
필요한 연산:
	•	find(u): u를 포함하는 집합 반환
	•	union(u, v): u와 v가 속한 집합을 병합
#### example
![[Pasted image 20250512152423.png|300]]
![[Pasted image 20250512152439.png|300]]
![[Pasted image 20250512152454.png|300]]
![[Pasted image 20250512152506.png|300]]
![[Pasted image 20250512152519.png|300]]
![[Pasted image 20250512152540.png|300]]
![[Pasted image 20250512152552.png|300]]
740을 받아들이면, 사이클이 생겨버리므로 제외함
![[Pasted image 20250512152606.png|300]]
![[Pasted image 20250512152616.png|300]]
849을 받아들이면, 사이클이 생겨버리므로 제외함
![[Pasted image 20250512152629.png|300]]
867을 받아들이면, 사이클이 생겨버리므로 제외함
![[Pasted image 20250512152640.png|300]]
![[Pasted image 20250512152651.png|300]]
1090을 받아들이면, 사이클이 생겨버리므로 제외함
![[Pasted image 20250512152701.png|300]]
1121을 받아들이면, 사이클이 생겨버리므로 제외함
![[Pasted image 20250512152712.png|300]]
모든 정점이 연결되었으므로 종료