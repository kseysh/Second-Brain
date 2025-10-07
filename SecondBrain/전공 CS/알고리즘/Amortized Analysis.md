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