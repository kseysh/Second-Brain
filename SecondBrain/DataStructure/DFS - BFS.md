## Graph traversal(G=( V , E ))
1. mark all vertices "unvisited"
2. for each v ∈ V
		if v is "unvisited"
		 do Traversal(G,v)
	  >=> (DFS, BFS) connected component마다 수행
		 
# DFS (Depth first search)
그래프를 탐색하기 위한 일반적인 기술
그래프의 모든 정점과 간선을 방문한다.
#### Graph의 DFS traversal
* G가 connected인지 결정한다.
	=> DFS를 하면서 확인할 수 있기 때문
* G의 connected components의 수를 계산한다.
	=> connected components 하나당 DFS 한 번
* G의 spanning forest의 수를 계산한다.
	=> DFS를 하면 spanning tree가 생긴다.
* `O(n+m)` Time을 지닌다.
	=> 모든 정점과 간선을 방문하기 때문
#### Graph problem을 풀기 위한 확장된 방법 : DFS
* 두 정점 사이의 경로를 찾아 알려줄 수 있다.
* 그래프가 cycle인지 acycle인지 구별해준다.
* binary tree에서의 오일러 투어와 같다.
  
  ### DFS Algorithm
  
```
Algorithm DFS_Traverse(G)
Input값은 graph G = (V, E)
	for all u ∈ V
		mark u as UNVISITED
	for all v ∈ V
		if v is marked as UNVISITED
		DFS(G, v)
```
  
  ```
Algorithm DFS(G, v)
Input값은 graph G = (V, E), and a start vertex v ∈ V

	mark v as VISITED
	for all w ∈ {neighbors of v}
		if w is marked as UNVISITED
			DFS(G,w)
```

### Examples


![[KakaoTalk_20230522_155527895.jpg|400]]
![[KakaoTalk_20230522_155550019.jpg|600]]
### Stack으로 표현
![[KakaoTalk_20230522_155653726.jpg|400]]

> DFS tree : DFS를 통해 graph를 순회한 순서대로 만든 tree, 
> DFS tree를 통해 back edge가 왜 back edge인지 알 수 있다. (DFS 트리에서 위로 올라가려는 반대 방향의 edge이므로 )


### DFS의 속성
1. DFS는 v의 connected component에서의 모든 간선과 정점을 방문한다.
2. 그래프 G에서 DFS를 사용하여 노드 v를 탐색할 때 발견된 간선들이 해당 connected component의 spanning tree를 형성한다.

### DFS 분석
* 각각의 정점은 두번 labeled 된다. ( UNVISITED => VISITED )
* 각각의 간선은 두번 labeled 된다. ( UNEXPLORED => DISCOVERY or BACK )
* `incidentEdges` method는 각 정점마다 한 번 호출된다.
* adjacency list로 구현된 그래프의 DFS는 `O(n+m)` time을 지닌다.
=>  Σ<span style="font-size:12px">v</span> deg(v) = 2m 를 호출하기 때문이다.

### Path Finding

그래프 G에서 v부터 z까지의 Path를 구하는 알고리즘
```
Algorithm pathDFS(G, v, z)
Input graph G = (V, E) and a start vertex v ∈ V
	S.push(v)
	mark v as VISITED
	if v=z
		return S.elements()
	for all w ∈ { neighbors of v }
		if w is marked as UNVISITED
			pathDFS(G, w, z)
	S.pop()
```

### Cycle Finding

그래프 G에서 v의 Cycle을 구하는 알고리즘
```
Algorithm cycleDFS(G, v)
	S.push(v)
	mark v as VISITED
	for all w  ∈ { neighbors of v }
		if(w,v) is UNEXPLORED
			if w is marked as UNVISITED
				mark (w,v) as EXPLORED
				cycleDFS(G,w)
			else
				T <- new empty stack
				while (o!=w)
					o <- S.pop()
					T.push(o)
				return T.elements()
	S.pop(v)
```
UNEXPLORED인데 VISITED이라면 사이클이 만들어진 것이므로 return을 시행한다.

=> 스택 s가 ECBA 순서로 pop되는 것을 스택 T로 바꿔 ABCE 순서대로 pop되도록 만든다.
graph

![[KakaoTalk_20230522_163031510.jpg|200]]
example을 보며 따라해보자!

# BFS (Breath first search)
#### Graph의 BFS traversal
그래프의 모든 정점과 간선을 방문한다.
그래프가 connected인지 판단한다.

### BFS Algorithm

  ``` 교수님 pseudo code
Algorithm BFS(G, v)
Q.enqueue(v)
mark v as "visited"
while (Q is not empty):
	u<-Q.dequeue()
	for each w ∈ neighbors(u):
		if w is unvisited:
			Q.enqueue(w)
			mark w as "visited"
```
모든 노드가 한 번씩 enqueue 되고, dequeue 된다.
enqueue하면서 visit, dequeue하면서 neighbors 훑기
DFS는 스택, BFS는 Queue를 이용한다.

  ``` 강의노트 pseudo code
Algorithm BFS(G, s)
L0<-new empty sequence
L0.insertBack(s)
mark s as VISITED
i<-0
while Li is not empty
	Li+1 <- new empty sequence
	for all v ∈ Li.elements()
		for all w ∈ {neighbors of v}
			if w is marked as UNVISITED
				mark w as VISITED
				Li+1.insertBack(w)
	i <- i+1
```

```
Algorithm BFS_Traverse(G)
Input graph G = (V, E)
	for all u ∈ V
		mark u as UNVISITED
	for all v ∈ V
		if v is marked as UNVISITED
		BFS(G, v)
```
  DFS_Traverse와 같다.

### Examples

![[KakaoTalk_20230527_195527480.jpg|400]]
![[KakaoTalk_20230527_195731705.jpg|400]]
![[KakaoTalk_20230527_195755850.jpg|400]]

> 왜 cross edge라고 부를까?


### BFS의 속성
G<span style="font-size:9px">s</span> : s의 Connected component
1. BFS(G, s)는 G<span style="font-size:9px">s</span>의 모든 정점과 간선을 방문한다.
2. BFS(G, s)로 표시된 discovery edge는 G<span style="font-size:9px">s</span>의 spanning tree를 형성한다.
3. Li 안의 각각의 정점 v는
	* s에서부터 v까지의 경로 T<span style="font-size:9px">s</span>는 i개의 간선을 가진다.
	* G<span style="font-size:9px">s</span> 안에서 s에서부터 v까지의 모든 경로는 적어도 i개의 간선을 갖는다.
	=> shortest path로 경로가 구성된다.

### BFS 분석
* 간선/정점 마킹은 O(1) Time이 소요된다.
* 각 정점은 두 번 mark 된다,
	* 첫 mark는 UNEXPLORED
	* 두 번째 mark는 VISITED로
* 각 간선은 두 번 방문된다. ??
* 각 정점은 L<span style="font-size:11px">i</span> 에 한 번 삽입된다.
* neighbors() 는 각 정점마다 한 번씩 호출된다.
* 그래프가 adjacency list structure로 표현되는 경우 BFS는 `O(n+m)` Time 에 동작한다. 

### BFS 활용
O(n+m) time이 걸린다 => 모든 정점과 간선을 순회하므로

그래프의 connected components의 수를 계산한다.
그래프의 spanning forest의 수를 계산한다.

두 정점의 최소 path를 찾을 수 있다. / 혹은 경로가 존재하지 않는다는 것을 전달한다.
simple cycle을 찾을 수 있다/ 그래프가 forest라는 것을 전할 수 있다.


### DFS vs BFS

![[KakaoTalk_20230527_203357541.jpg|400]]
BFS만 Shortest path를 찾을 수 있다.

![[KakaoTalk_20230527_203427050.jpg|400]]

BFS의 cross edge의 level number는 최대 1이 차이난다.



연관된 글
[[Graphs]]