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

![[Pasted image 20250528135546.png|200]]
A에서 출발 (B, C, F 방문)
B (D 방문)
D (C 방문)
C (Finished, stack push)
D (Finished, stack push)
B (Finished, stack push)
F (Finished, stack push)

다시 E에서 출발 (G 방문)
G (Finished, stack push)
E (Finished, stack push)
=> 이렇게 stack (a) 완성
![[Pasted image 20250501154729.png|400]]
- stack pop: E -> G 연결
- stack pop: G -> 이미 방문되어 있어 pass
- stack pop: A -> D, F 연결
	- D: B 연결 G는 visited라 pass
	- F: A는 visited라 pass
- stack pop: F -> visited라 pass
- stack pop: B -> A는 visited라 pass
- stack pop: D -> D는 visited라 pass
- stack pop: C -> A,B,D,E,F는 visited라 pass