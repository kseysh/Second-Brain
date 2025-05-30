## 목차
클래스 P와 클래스 NP
클래스 NP-완전
NP-완전 문제들
## 문제와 알고리즘
- 문제의 복잡도란 그 문제를 푸는 가장 좋은 알고리즘의 복잡도를 의미함
	- 정렬되지 않은 배열에서 최댓값을 찾는 문제는 Ω(n)
	- 정렬되지 않은 배열을 정렬하는 문제는 Ω(n log n)
	- “가장 좋음”은 입력이나 입력 순서에 따라 평균, 최악, 최선의 경우로 달라질 수 있음
## 최적화 문제 vs 결정 문제
- 최적화 문제: 최적의 값을 가지는 해를 찾는 문제
	- 예: SHORTEST-PATH
		- 그래프 G와 정점 u, v가 주어질 때, u에서 v까지 가장 적은 간선을 사용하는 경로를 찾는 문제
- 결정 문제: 답이 예/아니오로 주어지는 문제
	- 예: PATH
		- 그래프 G와 정점 u, v, 그리고 정수 k가 주어질 때, u에서 v까지 간선 수가 k 이하인 경로가 존재하는가?
## Polynomial Time: 클래스 P
- 어떤 알고리즘이 T(n) = O(n<sup>k</sup>) (단, k는 상수)라면 다항 시간 알고리즘이라 부름
- 어떤 문제에 대해 다항 시간 알고리즘이 존재하면 그 문제는 클래스 P에 속한다고 말함
	- 즉, 그 문제의 해를 다항 시간 안에 구할 수 있음
- 이 슬라이드에서는 다항 시간 알고리즘을 “효율적인” 알고리즘으로 간주
- 다항 시간은 지수 시간보다 훨씬 효율적임
## 어떤 문제들은 매우 어렵다
- 어떤 문제들은 현재 알려진 다항 시간 알고리즘이 존재하지 않음
	- 예: 0/1 배낭 문제, 외판원 순회 문제
- 결정 불가능한 문제들도 존재함
	- 예: 어떤 튜링 기계가 멈추는지 영원히 실행되는지 판별하는 알고리즘은 존재하지 않음
## 어려운 문제를 다루는 방법
- 어떤 문제가 어려워 보일 때 우리가 할 수 있는 것들
- 간혹 강한 하한 복잡도를 증명할 수 있음 (하지만 보통 어렵다)
- 집단적으로 문제의 어려움을 증명
## Non-Deterministic Polynomial Time: 클래스 NP
- 어떤 문제에 대해 비결정론적 다항 시간 알고리즘이 존재한다면, 그 문제는 클래스 NP에 속함
	- 즉, 해가 주어졌을 때 그 해가 맞는지 다항 시간 안에 검증할 수 있음
- 따라서 클래스 P에 속하는 문제는 항상 NP에도 속함 (해를 구할 수 있다면 당연히 검증도 가능)
	- 즉, P는 NP의 부분집합
	- 가장 큰 미해결 질문 중 하나: P = NP 인가?
	- 대부분의 연구자들은 P와 NP가 다르다고 생각함
NP: 정답이 맞는지만 확인하는 건 다항 시간에 할 수 있는 문제들의 모음
## NP 문제 예시
- 문제: 어떤 그래프가 가중치 K 이하의 최소 신장 트리를 가지는가?
- 검증 알고리즘:
	1.	증명으로서 n-1개의 간선 집합 T를 제시
	2.	T가 스패닝 트리인지 확인
	3.	T의 가중치가 K 이하인지 확인
- 분석: 검증은 O(n+m) 시간 안에 가능하므로 다항 시간에 수행됨

문제: 수가 들어 있는 리스트가 있고,
이 중에서 합이 10이 되는 두 수가 있는가?
•	이걸 직접 찾는 건 어려울 수 있어요. (예: 경우의 수가 많다면 오래 걸림)
•	하지만 누군가가 “5랑 5가 답이야!” 라고 제시하면?
•	→ “응, 5 + 5 = 10” → 바로 확인할 수 있어요. ✅
✅ 이런 문제는 NP 문제
## NP-완전 클래스
- 문제 Q가 NP-hard라면, 모든 NP 문제들을 다항 시간 안에 Q로 변환(reduce)할 수 있다
	- Q를 풀 수 있게 된다면, 모든 NP 문제를 그것을 이용해 풀 수 있다.
	- 모든 NP 문제들보다 같거나 더 어려운 문제들”의 집합
- 문제 Q가 NP-complete란, Q가 NP에 속하면서 동시에 NP-hard임을 의미
- 문제 P가 문제 Q로 환원 가능하다는 것은, 다항 시간 환원 함수 T가 존재하여
	- 모든 입력 x에 대해
		- x가 P에 대해 “예”라면 T(x)는 Q에 대해서도 “예”
		- x가 P에 대해 “아니오”라면 T(x)는 Q에 대해서도 “아니오”
		- T는 다항 시간 안에 계산 가능해야 함
## 다항 시간 환원
- 문제 P를 문제 Q로 환원 가능하면 P ≤p Q
	- P의 입력을 Q의 입력으로 변환
- 환원 관계는 추이성을 가짐
![[Pasted image 20250521180822.png|300]]
•	우리가 풀고 싶은 문제는 P이고, 입력은 x
•	x를 다항 시간 안에 변환 함수 T를 사용해 Q 문제의 입력 형태 T(x)로 바꿉니다.
•	이미 알고 있는 Q 문제를 푸는 알고리즘을 사용해 T(x)에 대해 Yes/No를 구합니다.
•	이 결과는 x에 대한 P 문제의 정답과 같도록 설계됨 → 이게 환원

문제 A가 NP-complete라고 알려져 있고	문제 A를 문제 B로 다항 시간에 환원 가능하다면
→ 문제 B는 NP-hard임을 증명할 수 있다.

“어떤 문제 P를 다항 시간에 문제 Q로 변환할 수 있다면, Q를 푸는 알고리즘을 이용해서 P도 풀 수 있다
## 최초의 NP-완전 문제
- 모든 NP-완전성 증명은 결국 CIRCUIT-SAT 문제로부터의 환원에 근거함
- CIRCUIT-SAT: AND, OR, NOT 게이트로 구성된 불 논리 회로가 주어졌을 때, 이 회로가 참이 되는 입력이 존재하는가?
![[Pasted image 20250521180848.png|300]]
## 일부 NP-완전 문제들
- SET-COVER: m개의 집합이 주어질 때, 이들 중 K개의 집합의 합집합이 전체 m개의 집합과 동일한가?
	- VERTEX-COVER 문제로부터의 환원으로 NP-완전
- SUBSET-SUM: 정수 집합과 정수 K가 주어졌을 때, 합이 K가 되는 부분집합이 존재하는가?
	- VERTEX-COVER로부터의 환원으로 NP-완전
## 또 다른 NP-완전 문제들
- 0/1 배낭 문제: 무게와 이익이 있는 아이템들이 주어졌을 때, 무게의 합이 W 이하이고 이익의 합이 K 이상인 부분집합이 존재하는가?
	- SUBSET-SUM으로부터 환원
- Hamiltonian-Cycle: 그래프 G가 있을 때, 모든 정점을 정확히 한 번씩 방문하는 순환 경로가 존재하는가?
	- VERTEX-COVER로부터 환원
- 외판원 순회 문제 (Traveling Salesperson Tour): 완전 가중 그래프 G가 주어졌을 때, 총 비용이 K 이하인 전체 정점을 순회하는 경로가 존재하는가?
	- Hamiltonian-Cycle로부터 환원
## 외판원 문제
완전 가중 그래프가 주어졌을 때, 최소 가중치의 투어(모든 정점을 방문하는 순환 경로)를 찾는 문제
![[Pasted image 20250521180953.png|150]]
## SAT
- bool 연산(0 또는 1)으로 이루어진 논리식
	- 예: (a+b+¬d+e)(¬a+¬c)(¬b+c+d+e)(a+¬c+¬e)
	- OR: +, AND: (곱), NOT: ¬
- SAT 문제: 논리식 S가 주어졌을 때, 0과 1을 변수에 적절히 할당하여 S를 참으로 만들 수 있는가?
	- CNF-SAT이 NP에 속함은 자명
		- 0과 1의 할당을 비결정론적으로 선택 후 각 절을 검사 모든 절이 참이면 전체 식도 참
	- SAT는 CIRCUIT-SAT로부터 환원되어 NP-완전
즉, S를 true로 만드는 변수들의 조합이 존재하냐는 것!
CNF-SAT: SAT 문제를 표준화된 형태로 바꾼 것
## 3SAT
- SAT 문제가 CNF이고, 각 절이 정확히 3개의 리터럴만 가지더라도 여전히 NP-완전
	- 예: (a+b+¬d)(¬a+¬c+e)(¬b+d+e)(a+¬c+¬e)
- SAT로부터의 환원
## Vertex Cover
- 그래프 G=(V,E)의 Vertex Cover란, V의 부분집합 W로서 모든 간선 (a,b)에 대해 a 또는 b 중 하나가 W에 속해야 함
- VERTEX-COVER 문제: 그래프 G와 정수 K가 주어졌을 때, 크기가 K 이하인 Vertex Cover가 존재하는가?
![[Pasted image 20250521181114.png|150]]
- VERTEX-COVER는 NP에 속함: 크기 K의 W를 비결정론적으로 선택 후 모든 간선이 W에 의해 커버되는지 확인
- VERTEX-COVER는 3SAT로부터의 환원으로 NP-완전
## P와 NP에 대한 고찰
- 일반적인 믿음: P는 NP의 진부분집합임
- 의미: NP-완전 문제들은 NP에서 가장 어려운 문제들임
- 이유: 만약 NP-완전 문제 중 하나라도 다항 시간에 풀 수 있다면, NP의 모든 문제도 다항 시간에 풀 수 있음
- 즉, NP-완전 문제가 다항 시간에 풀릴 수 있다면 P = NP
- 수많은 사람들이 NP-완전 문제에 대해 다항 시간 해법을 찾으려 했으나 실패했기 때문에, 어떤 문제가 NP-완전임을 보이는 것은 “수많은 똑똑한 사람들이 도전했지만 아직 해법이 없는 문제”라는 것을 의미함
![[Pasted image 20250521181146.png|300]]

## NP 증명
각 문제에 대해 NP에 속함을 증명하려면 다음을 보여야 한다
주어진 후보해(certificate)가 있을 때, 해당 문제의 조건을 만족하는지를 다항 시간 내에 검증할 수 있음.
1. SET-COVER 문제
	•	입력: 집합들의 컬렉션 S = {S₁, S₂, …, Sₙ}, 전체 원소의 집합 U, 정수 k
	•	질문: S의 부분집합 중 크기 ≤ k인 부분집합 C가 존재하여 C의 합집합이 U를 덮는가?
	•	후보해: 크기 ≤ k인 집합 C ⊆ S
	•	검증 방법:
	1.	C의 크기가 k 이하인지 확인.
	2.	C에 포함된 집합들의 합집합이 U와 동일한지 확인.
	•	복잡도: 집합끼리의 합집합과 포함 여부는 다항 시간 안에 처리 가능.
✅ NP에 속함

2. SUBSET-SUM 문제
	•	입력: 정수 집합 S = {a₁, a₂, …, aₙ}, 정수 t
	•	질문: 부분집합 S’ ⊆ S가 존재해서 합이 t가 되는가?
	•	후보해: 부분집합 S’
	•	검증 방법:
	1.	S’이 S의 부분집합인지 확인.
	2.	S’의 원소 합이 t인지 계산.
	•	복잡도: 합 계산은 O(n)
✅ NP에 속함

3. 0/1 배낭 문제 (0/1 Knapsack)
	•	입력: 각 아이템의 무게 wᵢ와 가치 vᵢ, 총 용량 W, 최소 기대 가치 K
	•	질문: 무게 총합 ≤ W이면서 가치 총합 ≥ K인 아이템 집합 존재?
	•	후보해: 선택된 아이템의 집합 S
	•	검증 방법:
	1.	선택된 아이템들의 총 무게 ≤ W인지, 총 가치 ≥ K인지 계산
	•	복잡도: O(n)에서 처리 가능
✅ NP에 속함

4. Hamiltonian-Cycle 문제
	•	입력: 무향 그래프 G = (V, E)
	•	질문: 모든 정점을 정확히 한 번씩 방문하고 시작점으로 돌아오는 사이클 존재?
	•	후보해: 정점 순열 (v₁, v₂, …, vₙ, v₁)
	•	검증 방법:
	1.	순열이 V의 모든 정점을 포함하는지 확인
	2.	인접한 정점 쌍이 실제 간선인지 확인
	•	복잡도: O(n)

✅ NP에 속함

5. SAT 문제 (Boolean Satisfiability Problem)
	•	입력: Boolean 식 φ (CNF 형태)
	•	질문: φ를 참으로 만드는 변수 할당 존재?
	•	후보해: 각 변수에 대한 참/거짓 할당
	•	검증 방법:
	1.	식에 대입하여 전체 결과가 참인지 계산
	•	복잡도: 식의 길이 m에 대해 O(m)

✅ NP에 속함

6. VERTEX-COVER 문제
	•	입력: 그래프 G = (V, E), 정수 k
	•	질문: 크기 ≤ k인 정점 집합 C ⊆ V가 모든 간선을 커버하는가?
	•	후보해: 정점 집합 C
	•	검증 방법:
	1.	C의 크기가 k 이하인지 확인
	2.	모든 간선 (u, v)에 대해 u 또는 v가 C에 있는지 확인
	•	복잡도: O(|E|)

✅ NP에 속함

7. CIRCUIT-SAT 문제
	•	입력: 논리회로 C (AND, OR, NOT 등으로 구성)
	•	질문: 어떤 입력에 대해 출력이 참이 되도록 하는 입력 조합 존재?
	•	후보해: 입력 비트열
	•	검증 방법:
	1.	입력값을 회로에 넣고 출력이 참인지 계산
	•	복잡도: 회로의 크기에 대해 선형 시간에 계산 가능

✅ NP에 속함
