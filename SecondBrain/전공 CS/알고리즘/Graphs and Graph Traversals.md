## Directed graph
•	방향 그래프(또는 digraph)는 쌍 G = (V, E)로 정의됨.
•	V는 정점(vertex)들의 집합
•	E는 V의 원소들로 이루어진 *순서쌍들의 집합*
	•	정점(vertex)은 노드(node)라고도 함
	•	E의 원소는 간선(edge), 방향 간선(directed edge), 또는 호(arc)라고 함
	•	방향 간선 (v, w)에서 v는 꼬리(tail), w는 머리(head)
	•	(v, w)는 다이어그램에서 화살표 v → w로 나타냄
	•	간단히 vw로 표기함
## Undirected Graph
•	무방향 그래프는 쌍 G = (V, E)로 정의됨.
•	V는 정점들의 집합
•	E는 V의 서로 다른 원소들로 이루어진 *순서 없는 쌍들의 집합*
	•	정점은 노드라고도 함
	•	E의 원소는 간선(edge) 또는 무방향 간선(undirected edge)
	•	각 간선은 V의 두 원소로 이루어진 부분집합으로 간주됨
	•	{v, w}는 무방향 간선을 나타냄
	•	다이어그램에서는 v─w로 나타냄
	•	간단히 vw 또는 wv로 표기 가능
	•	간선 vw는 정점 v와 w에 부착되어 있다(incident)고 말함
## 가중치 그래프 (Weighted Graph)
가중치 그래프는 삼중 (V, E, W)로 정의됨.
	•	(V, E)는 방향 또는 무방향 그래프
	•	W는 E에서 실수 집합 R로 가는 함수
	•	간선 e에 대해, W(e)는 e의 가중치(weight)라고 함
![[Pasted image 20250501154003.png|300]]
## 그래프 표현 방법 (Graph Representations)
G = (V, E), n = |V|, m = |E|, V = {v₁, v₂, …, vₙ}
•	인접 행렬 표현(Adjacency matrix representation): n × n 행렬
•	인접 리스트 배열 표현(Array of adjacency lists): 배열과 리스트로 구성
#### example
![[Pasted image 20250501154034.png|400]]
![[Pasted image 20250501154055.png|400]]
## 추가 정의 (More Definitions)
•	부분 그래프 (Subgraph)
•	완전 그래프 (Complete graph)
•	인접 관계 (Adjacency relation)
•	경로, 단순 경로, 도달 가능성 (Path, simple path, reachable)
•	연결됨, 강하게 연결됨 (Connected, Strongly Connected)
•	사이클, 단순 사이클 (Cycle, simple cycle)
•	비순환 (Acyclic)
•	무방향 포리스트 (Undirected forest)
•	프리 트리, 무방향 트리 (Free tree, undirected tree)
•	루트 트리 (Rooted tree)
•	연결 성분 (Connected component)
## 그래프 순회 (Traversing Graphs)
그래프 문제 해결을 위한 대부분의 알고리즘은 각 정점과 간선을 검사하거나 처리함.
너비 우선 탐색(BFS)과 깊이 우선 탐색(DFS)은 모든 정점과 간선을 한 번씩 “방문”하는 효율적인 순회 전략임
## 방향 그래프의 깊이 우선 탐색 (DFS)
•	시작 정점을 선택하고, 거리 d = 0
•	정점은 시작 정점으로부터 거리 증가 순으로 방문됨
•	거리 d의 정점에서 거리 d+1의 인접 정점으로 가는 간선 하나를 탐색
•	그 다음은 거리 d+1에서 d+2로, 계속 탐색
•	새로운 정점이 없거나 막다른 길이 나오면, 한 단계 뒤로(backtrack) 가서 다른 간선을 탐색
•	시작 정점까지 되돌아오고, 더 이상 새로운 정점이 없으면 종료
#### example
![[Pasted image 20250501154246.png|300]]
## DFS 트리(Depth-First Search Tree)
![[Pasted image 20250501154314.png|400]]
간선의 분류:
•	트리 간선(tree edge), 되돌아가는 간선(back edge), 자손 간선(descendant edge), 교차 간선(cross edge)

1.	vw가 탐색될 때 w가 미발견된 정점이면, vw를 트리 엣지(tree edge)라고 부르고 v는 w의 부모가 된다.
2.	w가 v의 조상이면, vw를 후진 엣지(back edge)라고 부른다( vw를 포함한다.).
3.	w가 v의 후손이고 vw가 탐색되기 전에 w가 발견되었다면 vw를 후손 엣지(descendent edge) 혹은 다른 이름으로 전진 엣지(forward edge)라고 부른다.
4.	w는 v와 조상/후손 관계가 없으면, vw를 교차 엣지(cross edge)라고 한다.
## 방향 그래프의 너비 우선 탐색 (BFS)
•	시작 정점을 선택하고, 거리 d = 0
•	정점은 시작 정점으로부터 거리 증가 순으로 방문됨
•	거리 d의 정점에서 거리 d+1의 모든 인접 정점으로 가는 간선을 탐색
•	다음으로 거리 d+1에서 d+2로 가는 모든 간선을 탐색
•	새로운 정점이 없을 때까지 반복
#### BFS 예시
•	정점 A에서 시작, d = 0
	•	B, C, F 방문 → d = 1
	•	D 방문 → d = 2
•	정점 E에서 시작, d = 0
	•	G 방문 → d = 1
![[Pasted image 20250501154545.png|400]]
## Strongly Connected Components
•	Strongly Connected: 
	방향 그래프 G에서 모든 정점 쌍 (v, w)에 대해 v에서 w로 가는 경로가 존재할 때
•	Strongly Connected Components
	방향 그래프 G의 최대 강하게 연결된 부분 그래프
## Algorithm
•	1단계: G에서 DFS 수행 중,
	종료 시간 순으로 정점을 스택에 저장
•	2단계: G의 전치 그래프(Gᵀ)에서 DFS 수행
	스택에서 정점을 하나씩 꺼내며 탐색 시작
•	각 Strongly Connected Components는 탐색의 시작 정점(리더)으로 식별됨
![[Pasted image 20250501154729.png|400]]