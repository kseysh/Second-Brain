 개체들 간의 관계를 표현하는 자료구조로, 정점(vertex)과 간선(Edge)으로 구성된다.

> A Tree is a undirected simple graph without cycles

###### 정점 (vertex)
노드의 set
###### 간선 (Edges)
vertex쌍의 집합

### Edge Types
#### 방향/유향 그래프 (Directed edge)
v는 도착지를 의미한다.
따라서 (u, v) != (v, u)

#### 무방향/무향 그래프 (Undirected edge)

## 그래프 용어

![[graphs.jpg|300]]

* **End vertices**(or **endpoints** of an edge
	U,V는 간선 a의 endpoints 이다.
	=> 어떤 간선에 직접적으로 연결되어 있는 정점
* Edges **incident** on a vertex
	a,d,b는 V와 인접하다
	=> 특정 정점과 연결된 간선
* **Adjacent** vertices
	U와 V는 인접하다.
	어떤 정점과 직접적으로 연결된 정점
* **Degree** of a vertex
	X는 5 degree를 가진다.
	=> 어떤 정점에 연결된 간선의 개수
* **Parallel** edges
	h와 i는 평행하다.
	=> 같은 두 개의 정점 사이에 여러 개의 간선이 존재하는 경우
* **Self-loop**
	j는 self-loop이다.
	=> 시작 정점과 도착 정점이 동일한 경우

![[path.jpg|300]]

* **Path** : 간선과 정점이 번갈아 나타나는 순서, 정점에서 시작해 정점에서 끝난다.
* **Simple path** : 그래프에서 두 정점 사이를 이동할 때, 동일한 간선이나 정점을 중복해서 지나지 않고 한 번씩만 지나는 경로, ex) P = (V, X, Z)


* **Cycle** : 시작점과 끝점이 동일한 Path   
* **Simple Cycle** : 한 정점에서 시작하여 모든 간선과 정점을 딱 한 번씩만 지나는 사이클 ex) C = (V, X, Y, W, U, V)

n = 정점의 수
m = 간선의 수
deg(v) = 정점 v의 degree
라 할때, 다음을 만족한다.

1. Σ<span style="font-size:12px">v</span> deg(v) = 2m
	각 간선은 두번 카운트 되기 때문
2. m <= n(n-1)/2
	무방향그래프에서 self-loop와 중복된 간선이 없다고 할 때 만족한다.
	m은 최대 nC2이기 때문이다.

## Graph ADT의 Main Methods

정점과 간선은 위치이면서, element를 저장한다.
### Accessor methods
e = edge
v = vertex
**e.endVertices()** : 간선에 직접적으로 연결되어있는 정점들의 리스트를 반환
**e.opposite(v)** : 간선 e 위에 있는 v에 반대에 있는 정점을 반환
**u.isAdjacentTo(v)** : u와 v가 인접하다면(adjacent) true를 반환


### Update methods
**insertVertex(o)** : o를 저장한 정점을 삽입
**insertEdge(v,w,o)** : v와 w를 잇는 간선을 삽입하고 o를 저장
**eraseVertex(v)** : 정점 v를 지운다.
**eraseEdge(e=(v,w))** : v와 w를 잇는 간선 e를 지운다.

### Iterable collection methods
**neighbors(v)** : v와 근접한 정점의 리스트를 반환
**vertices()** : graph에서 모든 vertices의 리스트를 반환
**edges()** : 그래프에서 모든 edges의 리스트를 반환

## 그래프를 표현하는 3가지 주요한 방법
### Edge list
정점을 저장한 Vertex List와 
출발정점, 도착정점의 쌍을 요소로 갖는 Edge List로 구성하는 방법
그래프의 구조를 간단하게 표현할 수 있지만 간선의 개수에 비례하는 공간을 요구한다.

### Adjacency List 
각 정점을 key로 가지고 해당 정점과 인접한 정점들을 linked list로 표현하는 방법 
각 정점은 array or hash table로 구성되어 있어 indexing시 O(1)Time이 든다고 가정한다.
그래프의 크기에 비례하는 공간을 요구한다.
각 정점마다 인접한 정점들을 저장하여 특정 정점과 인접한 정점을 빠르게 찾을 수 있다.
정점과 인접한 정점을 구현되는 방법이 다양하다.

n+m << n^2 일때는 adjacency list가 매우 유리하다. (=sparse 일 때 유리)

### Adjacency Matrix
정점들을 행과 열로 표현하며, 해당 정점들 간의 연결 여부를 2차원 배열로 표현하는 방법이다.
정점과 간선의 개수에 비례하는 공간을 요구한다.
행렬의 (i,j)요소는 정점 i와 j사이의 간선 여부를 나타낸다.
무방향 그래프라면 adjacency matrix는 대칭적이다.
simple 그래프라면 대각선 요소는 0이다. self-loops 가 없을 것이므로
각 요소 A<span style="font-size:12px">ij</span>는 i 번째 꼭짓점에서 j번째 꼭짓점까지 간선에 의한 가중치를 나타낸다. 
관계는 자주 바뀌지만 정점은 자주 추가되지 않을 때 사용
=> edge에 관련한 연산이 상수 Time이기 때문이다.
=> 또한 isAdjacentTo연산이 상수 Time이다.


>dense와 sparse란?
>>dense :  정점의 수에 비해 엣지의 수가 많은 그래프
>>sparse : 정점의 수에 비해 엣지의 수가 적은 그래프

## 그래프를 표현하는 3가지 방법의 시간 복잡도

![[Performance.jpg|500]]

> Edge list 방식은 불필요하게 전부 확인을 한다. 따라서 insertion은 쉽지만 검색이 어렵다.
 
n = 정점의 수
m = 간선의 수
### Edge List
*space* : `O(n+m)`
**neighbors(v)** : `O(m)`
이웃을 찾기 위해 모든 Edge List를 순회하므로
**u.isAdjacentTo(v)** : `O(m)`
인접한지 확인하기 위해 모든 Edge List를 순회하므로
**insertVertex(o)** : `O(1)`
vertex list에 추가만 하면 되므로 상수 time
**insertEdge(v,w,o)** : `O(1)`
Edge list에 추가만 하면 되므로 상수 time
**eraseVertex(v)** : `O(n+m)`
vertex를 삭제하기 위해 vertex list에서 삭제할 값을 찾고,
삭제할 vertex와 연결된 edge를 삭제하기 위해 Edge list에서 삭제할 값을 찾는다.
**eraseEdge((u,v))** : `O(m)`
edge를 삭제하기 위해 Edge list에서 삭제할 값을 찾는다.
### Adjacency List
*space* : `O(n+m)`
정점을 저장하기 위한 space n과, n의 각 degree를 더한 값인 m(간선의 수)
**neighbors(v)** : `O(deg(v)≤n)`
v의 degree는 n보다 많을 수 없으므로 n보다 작다. 
v를 찾는 시간 상수 시간, v의 이웃을 확인하기 위해 deg(v)
**u.isAdjacentTo(v)** : `O(min(deg(u),deg(v)≤n)`
u와 v중 degree가 낮은 값을 찾고 degree가 낮은 노드에서 degree를 확인하며 u,v가 Adjacent인지 확인한다.
**insertVertex(o)** : `O(1)`
array 혹은 hashtable에 값을 하나 추가하는 것과 같으므로 상수시간
**insertEdge(v,w,o)** : `O(1)`
v와 w에 linked list를 한 개씩 추가하는 것과 같으므로 상수시간
**eraseVertex(v)** : v의 이웃의 deg를 모두 더한 값 ≤ m
v를 지우기 위해서는 v의 이웃과의 edge를 전부 지워야하므로, v의 degree 뿐만 아니라 v의 이웃의 인접리스트에서 v를 찾아 지워야 한다. 
삭제는 상수시간이므로 v의 이웃의 degree를 모두 더한 값이 최악시간이고, 이는 간선의 개수인 m보다 작다.
**eraseEdge((u,v))** : `O(deg(u)+deg(v)≤n)`
u의 인접리스트에서 v를 찾아서 지우고, v의 인접리스트에서 u를 찾아서 지워야 하므로 deg(u)와 deg(v)를 더한 값이며 이는 노드의 개수인 n보다 작다.

>neighbors의 cost가 낮은 것이 장점!

### Adjacency Matrix
*space* : `O(n²)`
matrix가 n²의 공간을 차지한다.
**neighbors(v)** : `O(n)`
v의 열or행을 확인하며 이웃을 확인한다.
adjacency matrix의 열과 행은 크기가 n이므로 O(n) time이 소요된다.
**u.isAdjacentTo(v)** : `O(1)`
(u,v)가 1인지 확인하면 되는 간단한 인덱싱이므로 상수시간이 소요된다.
**insertVertex(o)** : `O(n²)`
정점을 만들기 위해서는 새로운 matrix를 생성하여야 하므로 새로운 n²배열을 만들기 위한 시간 n²이 소요된다. 
**insertEdge(v,w,o)** : `O(1)`
edge를 추가하기 위해서는 (v,w)의 값을 1로 변경해주기만 하면 되는 간단한 연산이므로 상수 시간이 소요된다.
**eraseVertex(v)** : `O(n²)`
insertVertex와 비슷하게 정점을 지우기 위해서는 새로운 matrix를 생성하여야 하므로 새로운 배열을 만들기 위한 시간 n²이 소요된다.
**eraseEdge((u,v))** : `O(1)`
edge를 삭제하기 위해서는 (v,w)의 값을  0으로변경해주기만 하면 되는 간단한 연산이므로 상수 시간이 소요된다.

> 정점이 고정되어 있고, 간선이 자주 바뀔 때 유용하다.
> 정점 추가 삭제는 O(n²) 이지만 간선 추가 삭제는 O(1)이다.
> neighbors도 조금 cost가 높고, space가 높다.

## Subgraphs
G=(V,E) 일 때, S=(V',E')이 V'∈V, E'∈E, E'∈V'xV'
이라면, 어떤 것이든지 subgraph라고 부를 수 있다.
만약 edge가 없어도, 서로 연결이 되어있지 않아도 subgraph라고 부른다.

### spanning subgraph
V'=V인 경우로, 정점이 모두 존재하는 subgraph이다.

### induced subgraph
subgraph에서의 정점이 원래 graph에서 가지고 있는 모든 간선을 포함하는 subgraph이다. 
원래 그래프에서의 정점이 5개이고 subgraph의 정점이 3개라면 그 3개의 정점은 원래 그래프에서의 간선을 모두 가지고 있어야 한다.

### Connectivity
모든 노드가 서로 연결되어 어떤 임의의 노드를 두개 뽑더라도 갈 수 있는 경로가 존재하는 상태.

### Connected component
그래프에서 Connectivity를 지니는 그래프의 수

### Trees and Forests
**Tree** : connectivity이고, cycle를 지니지 않는(acyclic graph) undirected graph
rooted tree와는 다른 개념이다.
**Forests** : 그래프에 속하는 모든 Connected Component가 전부 tree인 경우

### Spanning Trees and Forests
**Spanning Tree** : 모든 노드를 포함하면서 cycle을 지니지 않으면서 connected된 undirected graph
**Spanning Forest** : 모든 노드를 포함하면서 acyclic한 undirected graph의 모임
