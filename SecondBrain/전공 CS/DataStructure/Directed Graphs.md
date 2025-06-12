## Digraphs
digraph는 directed graph를 줄인 말로 모든 edge들이 방향을 갖는 그래프이다.

### Digraph의 속성
* 각각의 edge는 한 방향만을 지닌다.
* edge(a,b) 는 a에서 출발해 b로간다. 
* 그래프가 중복되는 간선이나 자기 자신을 가리키는 간선이 없다면 즉, simple 하다면, `m ≤ n(n-1)`이다.
* digraph의 각 정점에 대해 해당 정점으로 들어오는 간선들과 해당 정점에서 나가는 간선들을 각각 별도의 인접 리스트로 관리한다면, 들어오는 간선과 나가는 간선의 목록을 해당 간선들의 크기에 비례하는 시간 안에 나열할 수 있다.

#### Digraph의 활용 -Scheduling
edge(a, b)는 b가 시작하기전 완료해야할 과제를 의미한다.


### Digraph에서의 DFS와 BFS

#### Directed DFS
![[KakaoTalk_20230527_212327958.jpg|500]]
Digraph의 방향에 따라서만 간선을 순회할 수 있도록 DFS나 BFS의 방식으로 traversal algorithm을 만들 수 있다 
Directed DFS는 시작 정점 s에서 출발하여 방문 가능한 모든 정점을 결정한다.
directed DFS algorithm에서는 4 type의 간선이 존재한다.
1. discovery edges
	* 실제로 순회하게 되는 edge이다.
2. back edges
	* discovery edge로 tree를 만들었을 때 조상에게 가는 edge이다.
3. forward edges
	*  discovery edge로 tree를 만들었을 때 자식에게 가는 edge이다.
4. cross edges
	*  discovery edge로 tree를 만들었을 때 사촌/형제에게 가는 edge이다.

#### Directed BFS
![[KakaoTalk_20230527_212350920.jpg]]
DFS와 다르게 directed BFS는 세 가지 type으로 간선을 분류한다.
1. discovery edges
	* 순회하며 실제로 들리는 edge
2. back edges
	* discovery edge로 tree를 만들었을 때 (직속)조상에게 가는 edge이다.
3. cross edges
	*  discovery edge로 tree를 만들었을 때 사촌(직속이 아닌 아래 level) / 형제에게 가는 edge이다.
directed BFS tree에는 forward edge가 존재하지 않는다.

#### Reachability
v를 root로 한 DFS/BFS tree에서 정점들은 directed path를 통해 v로 도달할 수 있다.

#### Strong Connectivity
각 정점이 모든 다른 정점들에 도달할 수 있을 때 Strong Connectivity라 한다.

```
Algorithm Strong Connectivity
그래프에서 정점 v를 택한다.
v에서부터 DFS를 수행한다.
	If w가 방문되지 않았다면 "no"를 출력한다.
G의 간선을 뒤집어서 G'를 만든다.
G'에서 DFS를 수행한다.
	If w가 방문되지 않았다면, "no"를 출력한다.
	Else, "yes"를 출력한다.
```
두 번의 DFS or BFS를 통해 Strong Connectivity를 확인 가능하다.
![[KakaoTalk_20230527_215711826.jpg|200]]
`O(n+m)`Time

#### DAGs (Directed acyclic graph)
최소 하나의 Source vertex와 최소 하나의 sink vertex가 있어야 한다.
Topological Ordering이 필요충분조건이다.
> source vertex : incoming edge를 가지지 않는 vertex
> sink vertex : outgoing edge를 가지지 않는 vertex
#### Topological Ordering
모든 (V<span style="font-size:11px">i</span>,V<span style="font-size:11px">j</span>)를 갖는 Edge 에서 i < j 여야 한다.

#### Topological sort
```
Algorithm TopologicalSort(G)
H<-G
n<-G.numVertices()
whlie H is not empty do
	Let v be a vertex with no outgoing edges
	Label v <- n
	n <- n-1
	Remove v from H
```

#### TopologicalDFS
```
Algorithm topologicalDFS(G)
	Input DAG G = (V,E)
	Output topological ordering of G
n<-V // n에 vertex의 수를 넣는다.
for all u ∈ V
	mark u as UNVISITED // 모든 vertex에 unvisited를 라벨링 함
for all v ∈ V
	if v is marked as UNVISITED 
		topologicalDFS(G,v)
	// unvisited들을 모두 topologicalDFS를 한다.

Algorithm topologicalDFS(G, v)
	Input graph G and a start vertex v of G
	Output labeling of the vertices of G 
	in the connected component of v

	mark v as VISITED
	for all w ∈ {neighbors of v}
		if w is marked as UNVISITED
			topologicalDFS(G,w)
		Label v with topological number n
		n <- n-1
```
Topological sort와 Topological DFS의 결과는 동일하다.




연관된 글
[[Graphs]]