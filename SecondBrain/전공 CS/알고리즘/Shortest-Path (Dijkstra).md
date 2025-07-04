## 문제: 단일 시작점 최단 경로
- 두 지정된 정점 사이의 최소 가중치 경로를 찾는다.
- 최악의 경우, 특정한 정점 쌍 s와 t 사이의 최소 가중치 경로를 찾는 것은 정점 s에서 도달 가능한 모든 정점까지의 최소 가중치 경로를 찾는 것보다 더 쉽지 않다.
	- 이것이 단일 시작점 최단 경로 문제이다.
## 최단 경로 정의
 -  그래프 G = (V, E, W)에서 P를 k개의 간선으로 구성된 비어 있지 않은 경로라고 하자. 이 경로는 xv<sub>1</sub>, v<sub>1</sub>, v<sub>2</sub>, ...., v<sub>k-1</sub>y (여기서 v<sub>1</sub> = y일 수도 있음)로 이루어져 있다.
- 경로 P의 가중치 W(P)는 각 간선의 가중치 W(xv<sub>1</sub>), W(v<sub>1</sub>v<sub>2</sub>), ..., W(v<sub>k-1</sub>y)의 합이다.
- 만약 x = y라면, 빈 경로(empty path)는 x에서 y로 가는 경로로 간주한다. 이때 빈 경로의 가중치는 0이다.
- x에서 y까지의 경로 중 W(P)보다 더 작은 가중치를 가진 경로가 없다면, P는 최단 경로(shortest path) 또는 최소 가중치 경로(minimum-weight path) 라고 한다.
## 최단 경로의 성질
보조정리: 최단 경로 성질
- 가중치 그래프 G에서 x에서 z로 가는 최단 경로가 x에서 y까지의 경로 P와 y에서 z까지의 경로 Q로 구성되어 있다면,
- P는 x에서 y까지의 최단 경로이고 Q는 y에서 z까지의 최단 경로이다.
## Dijkstra's Shortest-Path Algorithm
Weight는 음수가 아님
![[Pasted image 20250512153135.png|400]]
**빵꾸 메우기**
1. 모든 정점을 unseen으로 초기화
2. 시작 정점 s를 tree로 지정하고, d(s, s) = 0
3. s와 인접한 정점들을 fringe로 재분류
4. while (fringe 정점이 존재하는 동안)
	1. tree 정점 t와 fringe 정점 v 간의 edge 중 (d(s, t) + W(tv)값이 최소인 (t, v)선택)
	2. v를 tree로 재분류, edge (t, v)를 트리에 추가
	3. v와 인접한 unseen 정점들을 fringe로 재분류

![[Pasted image 20250512153145.png|400]]
A에서 목적지 어딘가로 가는 shortest path를 찾고자 하는 문제임
## 정당성 (Correctness)
**나올 가능...성만 있음**
### 정리
- G=(V,E,W)가 가중치가 모두 0 이상인 그래프라고 하자.
- V’는 V의 부분 집합이며, s는 V’에 포함된 정점이다.
- 모든 y ∈ V’에 대해, d(s,y)는 s에서 y까지의 최단 거리라고 하자.
- 한 꼭짓점 y는 V’에, 다른 꼭짓점 z는 V - V’에 속하는 모든 간선 yz 중에서
d(s, y) + W(yz)를 최소로 하는 간선을 선택했을 때, s에서 y까지의 최단 경로와 간선 yz를 잇는 경로는 s에서 z까지의 최단 경로이다.
### 정리
- 방향성 가중 그래프 G와 음수가 아닌 가중치를 갖는 간선들, 그리고 시작 정점 s가 주어졌을 때, Dijkstra 알고리즘은 s로부터 도달 가능한 각 정점까지의 최단 거리를 계산한다.
#### 시간 복잡도
insert or decrease key: 우선 순위 큐를 사용하므로 연산당 비용 O(log n), 간선 수만큼 반복
가장 가까운 정점 꺼내기: 우선 순위 큐를 사용하므로 연산당 비용 O(log n), 정점 수만큼 반복
	따라서 Dijkstra 알고리즘 시간 복잡도 O((n + m)log n) = O(mlogn)