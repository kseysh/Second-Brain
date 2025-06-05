## Transitive Closure
방향 그래프 G가 주어졌을 때, G의 transitive closure(추이 폐쇄)는 다음 조건을 만족하는 방향 그래프 G\*이다.
	•	G\*는 G와 동일한 정점을 갖는다.
	•	G에 정점 u에서 v로 가는 경로가 존재한다면 (단, u ≠ v), G\*에는 u에서 v로 가는 간선이 존재한다.

Transitive closure는 방향 그래프의 도달 가능성 정보를 제공한다.
![[Pasted image 20250521165334.png|100]]
## Transitive Closure 계산하기
각 정점에서 DFS를 수행하면 된다.
시간 복잡도는 O(n(n + m))이다.

만약 A에서 B로, B에서 C로 갈 수 있다면,
A에서 C로 가는 방법도 존재한다.

다른 방법으로는 동적 프로그래밍을 사용할 수 있다.
→ Floyd-Warshall 알고리즘
## Floyd-Warshall Transitive Closure
아이디어 1: 정점들을 1, 2, …, n으로 번호 매긴다.
아이디어 2: 1부터 k까지 번호 매겨진 정점만 중간 정점으로 사용하는 경로를 고려한다.
	•	정점 1,…,k만 사용하는 경로 (간선이 없으면 새로 추가)
	•	정점 1,…,k-1만 사용하는 경로
	•	정점 1,…,k-1만 사용하는 경로
![[Pasted image 20250521165525.png|300]]
## Floyd-Warshall's Algorithm
![[Pasted image 20250521165556.png|200]]
i: v의 번호
j, k: v<sub>i</sub>와 인접한 서로 다른 정점 번호 
Floyd-Warshall 알고리즘은 G의 정점을 v1, …, vn으로 번호 매기고
일련의 방향 그래프 G0, …, Gn을 계산한다.
	•	G0 = G
	•	Gk는 G에 대해 {v1,…,vk} 집합 내 정점만 중간 정점으로 사용하는 경로가 존재할 경우 vi에서 vj로 가는 간선을 가진다.
결과적으로 Gn = G\*가 된다.
k단계에서 Gk는 Gk−1에서 계산된다.
실행 시간은 O(n³)이며, areAdjacent 연산이 O(1)이라면 (예: 인접 행렬 사용 시) 해당 복잡도가 유지된다.
#### Example
![[Pasted image 20250521165702.png|400]]
v1 부터 시작
v1이 시작 정점: (v1,v4) 존재
v1이 도착 정점: (v5, v1), (v3, v1)
![[Pasted image 20250521165712.png|400]]
따라서, (v5,  v4)에 대해 연결, (v3, v4)는 이미 있으므로
![[Pasted image 20250521165723.png|400]]
v2는 나가는 정점이 없어 pass
![[Pasted image 20250521165737.png|400]]
v3이 시작 정점: (v3, v1), (v3, v2), (v3, v4)
v3이 도착 정점: (v4, v3), (v5, v3), (v6, v3)
따라서, 위 edge들을 추가
![[Pasted image 20250521165750.png|400]]
v4의 경우 똑같음
![[Pasted image 20250521165801.png|400]]
v5도 똑같음
![[Pasted image 20250521165817.png|400]]
v6는 이미 연결되어 있으므로 pass
![[Pasted image 20250521165828.png|400]]
v7도 이미 연결되어 있으므로 pass
## 모든 정점 쌍 간 최단 거리 (All-Pairs Shortest Paths)
![[Pasted image 20250521165848.png|300]]
가중 방향 그래프 G에서 모든 정점 쌍 사이의 거리를 구한다.
•	음수 간선이 없으면 Dijkstra 알고리즘을 n번 호출 가능 → 시간 복잡도 O(nm log n)
•	Floyd-Warshall 알고리즘처럼 동적 프로그래밍을 사용하면 → O(n³) 시간 복잡도 달성 가능
![[Pasted image 20250521170012.png|300]]

**앞에거랑 어떤 차이가 있는지**
**Complexity 알아두기**