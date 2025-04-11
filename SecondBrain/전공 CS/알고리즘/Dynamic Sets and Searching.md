## Array Doubling
한 요소를 복사하는데 드는 비용: t
- (n+1)번째 요소 삽입이 배열 확장을 유발할 경우
	- 이 배열 확장 작업의 비용은 t * n
	- 이전 배열 확장들의 비용은 t * n/2 + t * n/4 + t * n/8 + ...로, 이는 t * n보다 작음
- 총 비용은 2t * n을 초과하지 않음
## Amortized Analysis (분할 상환 분석)
- 최악의 경우에 대해 **확률적 가정을 하지 않고** 평균 비용을 논하는 방법
- Accounting method (회계 방식)
	- **amortized cost = actual cost + accounting cost** (시)
	- 다음 두 가지 조건을 만족해야 함:
		1. 어떤 유효한 연산 순서에 대해서도 accounting cost의 총합은 음수가 아니어야 함
		2. 각 연산의 amortized cost을 분석할 수 있도록 해야 함
## Array Doubling 사용하는 스택 구현
•	필요에 따라 배열을 확장하기 위해 내부적으로 Array Doubling을 사용함
	•	배열 확장이 일어나지 않을 때, push나 pop의 실제 비용은 1
	•	배열 확장이 필요한 경우, push의 실제 비용은 1 + t * n
		n: 현재 배열에 있는 원소 개수, t: 복사할 때 한 요소당 드는 비용
•	회계 비용 할당 방식:
	•	배열 확장이 없을 때 push의 Accounting cost는 2t로 설정
	•	배열 확장이 있을 때 push의 Accounting cost는 –t * n + 2t로 설정
•	각 push 연산의 amortized cost은 1 + 2t
•	스택이 생성된 시점부터 Accounting cost의 총합은 절대 음수가 되어서는 안 됨
## Binary Search Tree
이진 탐색 트리를 inorder traversal하면 키들이 정렬된 리스트로 출력된다.
![[Pasted image 20250410155125.png|300]]
dot: 빈 tree

![[Pasted image 20250410155036.png|300]]
### Binary Search 분석
•	키를 검색할 때 조사되는 내부 노드의 개수 n을 기준으로 분석
•	트리 구조가 긴 체인 형태일 경우: θ(n)의 시간 소요
•	트리가 최대한 균형 잡혀 있을 경우: θ(log n)
•	목표는 트리를 최대한 균형 있게 만드는 것
	•	이를 위한 기술: 이진 트리 회전(Binary Tree Rotations)
## Red-Black Tree
레드-블랙 트리는 다음의 성질들을 만족하는 이진 탐색 트리로 정의할 수 있다: (시)
•	루트 속성: 루트는 검정색이다.
•	외부 속성: 모든 리프 노드는 검정색이다.
•	내부 속성: 빨간 노드의 자식은 모두 검정색이다.
•	깊이 속성: *모든 리프 노드는 같은 검정색 깊이(black depth)를 가진다.*
### Height of a Red-Black Tree
•	정리: n개의 항목을 저장하는 레드-블랙 트리의 높이는 O(log n)이다.
•	레드-블랙 트리에서의 검색 알고리즘은 일반 이진 탐색 트리와 동일하다.
•	위 정리에 따라, 레드-블랙 트리에서의 검색 시간은 O(log n)이다.
레드블랙트리의 성질상 전체 높이가 **최대 2 × log₂(n)** 이하로 유지
### 삽입 (Insertion)
•	insertItem(k, o) 연산을 수행하기 위해, 일반 이진 탐색 트리의 삽입 알고리즘을 실행하고 새로 삽입된 노드 z를 루트가 아닌 이상 빨간색으로 색칠한다.
•	루트, 외부, 깊이 속성을 유지해야 한다.
•	만약 z의 부모 v가 검정색이라면, 내부 속성도 만족하므로 종료된다.
•	그러나 v가 빨간색이라면, double red가 발생하여 내부 속성이 위배되므로 트리의 재구성(reorganization)이 필요하다.
#### example 
값 4의 삽입이 double red를 유발하는 경우
![[Pasted image 20250410161145.png|300]]
### Double Red 해결
•	자식 노드 z와 부모 v 간의 이중 빨강 상황에서 v의 형제 노드를 w라고 할 때 다음 두 가지 경우가 있다:
경우 1: w가 검정색인 경우
	•	재구조화(Restructuring) 수행
![[Pasted image 20250410161458.png|100]]
경우 2: w가 빨간색인 경우
	•	재색칠(Recoloring) 수행
![[Pasted image 20250410161514.png|100]]
### Restructuring
•	부모 v가 검정색 형제 w를 가지고 있는 이중 빨강 상황에서 수행
•	내부 속성을 회복하고, 나머지 속성들도 유지됨
• 이중 빨강 노드들이 왼쪽 자식인지 오른쪽 자식인지에 따라 총 4가지 재구조화 구성이 존재함
![[Pasted image 20250410162456.png|200]]
![[Pasted image 20250410162522.png|300]]
## Recoloring
•	부모 v가 빨간색 형제 w를 가지고 있는 double red 상황에서 수행
•	v와 w를 검정색으로 바꾸고, 할아버지 노드 u를 빨간색으로 만듦 (단, u가 루트가 아닌 경우에 한함)
•	double red violation이 할아버지 노드 u로 전파될 수 있음
![[Pasted image 20250410162627.png|300]]

### 수도 코드
![[Pasted image 20250410162823.png|200]]
1. 키 k를 이용해 삽입 위치 노드 z를 찾는다
	•	새로운 항목을 넣기 위해 이진 탐색 트리 방식으로 위치를 탐색함
	•	이때 z는 삽입될 노드 위치임

2. (k, o)를 노드 z에 추가하고 z를 빨간색으로 칠한다
	•	Red-Black Tree에서는 새로 삽입되는 노드는 항상 빨간색으로 시작함
	•	이는 트리의 성질을 보존하기 위한 시작점

3. doubleRed(z)가 참인 동안 반복 (즉, z와 부모 노드가 모두 빨간 경우)
	•	이 상황은 Red-Black Tree의 규칙을 위반하므로 수정이 필요함

3.1. parent(z)의 형제가 검정색이면
	•	z ← restructure(z) 수행 (회전 연산)
	•	구조적 변형을 통해 균형을 맞추고 규칙 위반을 해결함
	•	이후에는 return하여 알고리즘 종료

3.2. parent(z)의 형제가 빨간색이면
	•	색깔 재조정: z ← recolor(z)
	•	부모와 삼촌이 모두 빨간 경우, 조상 쪽으로 문제를 전파하며 색상만 바꿈
#### example
4 7 12 15 3 5 14 18 16 17 순서로 삽입
![[Pasted image 20250410162946.png|300]]
왼쪽 아래 -> 15의 입장에서 부모 12는 빨간색 형제 4를 가짐 -> Recoloring
원래 7은 빨간색으로 바뀌어야 하지만, root라서 검정색 됨

![[Pasted image 20250410162955.png|300]]
![[Pasted image 20250410163011.png|300]]
![[Pasted image 20250410163029.png|300]]
![[Pasted image 20250410163042.png|300]]
