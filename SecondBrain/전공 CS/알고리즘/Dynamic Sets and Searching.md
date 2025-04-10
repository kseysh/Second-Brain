## Array Doubling
한 요소를 복사하는데 드는 비용: t
- (n+1)번째 요소 삽입이 배열 확장을 유발할 경우
	- 이 배열 확장 작업의 비용은 t * n
	- 이전 배열 확장들의 비용은 t * n/2 + t * n/4 + t * n/8 + ...로, 이는 t * n보다 작음
- 총 비용은 2t * n을 초과하지 않음
## Amortized Analysis (분할 상환 분석)
- 최악의 경우에 대해 **확률적 가정을 하지 않고** 평균 비용을 논하는 방법
- Accounting method (회계 방식)
	- amortized cost = actual cost + accounting cost
	- 다음 두 가지 조건을 만족해야 함:
		1. 어떤 유효한 연산 순서에 대해서도 accounting cost의 총합은 음수가 아니어야 함
		2. 각 연산의 amortized cost을 분석할 수 있도록 해야 함
## Array Doubling 사용하는 스택 구현
•	필요에 따라 배열을 확장하기 위해 내부적으로 Array Doubling을 사용함
	•	배열 확장이 일어나지 않을 때, push나 pop의 실제 비용은 1
	•	배열 확장이 필요한 경우, push의 실제 비용은 1 + t * n
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
레드-블랙 트리는 다음의 성질들을 만족하는 이진 탐색 트리로 정의할 수 있다:
•	루트 속성: 루트는 검정색이다.
•	외부 속성: 모든 리프 노드는 검정색이다.
•	내부 속성: 빨간 노드의 자식은 모두 검정색이다.
•	깊이 속성: 모든 리프 노드는 같은 검정색 깊이(black depth)를 가진다.
### Height of a Red-Black Tree
•	정리: n개의 항목을 저장하는 레드-블랙 트리의 높이는 O(log n)이다.
•	레드-블랙 트리에서의 검색 알고리즘은 일반 이진 탐색 트리와 동일하다.
•	위 정리에 따라, 레드-블랙 트리에서의 검색 시간은 O(log n)이다.
### 삽입 (Insertion)
•	insertItem(k, o) 연산을 수행하기 위해, 일반 이진 탐색 트리의 삽입 알고리즘을 실행하고 새로 삽입된 노드 z를 루트가 아닌 이상 빨간색으로 색칠한다.
•	루트, 외부, 깊이 속성을 유지해야 한다.
•	만약 z의 부모 v가 검정색이라면, 내부 속성도 만족하므로 종료된다.
•	그러나 v가 빨간색이라면, 이중 빨강(double red)이 발생하여 내부 속성이 위배되므로 트리의 재구성(reorganization)이 필요하다.

#### example 
값 4의 삽입이 double red를 유발하는 경우
![[Pasted image 20250410161145.png|300]]