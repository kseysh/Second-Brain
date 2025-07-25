## 동적 프로그래밍 버전의 재귀 알고리즘
하위 문제의 해를 다시 계산하지 않고 저장함으로써 공간을 속도와 맞바꾼다.
하위 문제의 해를 찾을 때마다, 이를 soln이라는 딕셔너리에 기록한다.
	재귀 호출을 하기 전에, 예를 들어 Q라는 하위 문제에 대해 호출하기 전에, 딕셔너리 soln에 Q에 대한 해가 저장되어 있는지 확인한다.
	•	저장되어 있지 않으면 재귀 호출을 진행한다.
	•	저장되어 있으면 재귀 호출 없이 저장된 해를 반환한다.
	해를 반환하기 직전에 그 결과를 soln에 저장한다.
#### Example: fib
![[Pasted image 20250521172313.png|300]]
## 행렬 곱셈 최적화 (Matrix-Chain Multiplication)
n개의 행렬 <A<sub>1</sub>, A<sub>2</sub>, …, A<sub>n</sub>>이 주어졌을 때,
	A<sub>1</sub>A<sub>2</sub>…A<sub>n</sub>의 곱을 계산하자.
	곱셈은 결합 법칙이 성립한다.
		예: (A<sub>1</sub>×A<sub>2</sub>) × A<sub>3</sub> = A<sub>1</sub> × (A<sub>2</sub>×A<sub>3</sub>)
문제 정의:
n개의 행렬 <A<sub>1</sub>, A<sub>2</sub>, …, A<sub>n</sub>>이 주어졌을 때, 각 행렬 Ai는 p<sub>i-1</sub> × p<sub>i</sub> 크기를 가지며, 스칼라 곱셈 연산의 수를 최소화하도록 전체 곱셈 순서를 괄호로 묶어라.
## 계산 시간
p×q 행렬과 q×r 행렬을 곱하면,
p × q × r개의 스칼라 곱셈이 필요하다.
(→ pr개의 원소를 계산하며, 각 원소마다 q번 곱한다)
#### example
![[Pasted image 20250521172544.png|400]]
## 알고리즘 개발 단계
1.	최적 해의 구조를 파악한다.
2.	최적 해의 값을 재귀적으로 정의한다.
3.	최적 해의 값을 Bottom-up 방식으로 계산한다.
4.	계산된 정보를 바탕으로 최적 해를 구성한다.
## 1단계: 최적 괄호 묶기의 구조
A<sub>i</sub>A<sub>i+1</sub>…A<sub>j</sub>를 A<sub>i</sub>…A<sub>k</sub>와 A<sub>k+1</sub>…A<sub>j</sub>로 나누자 (i ≤ k < j)
비용 = A<sub>i..k</sub>의 계산 비용 + A<sub>k+1..j</sub>의 계산 비용 + 두 결과 곱셈 비용
A<sub>i</sub>…A<sub>k</sub>와 A<sub>k+1</sub>…A<sub>j</sub> 각각도 최적이어야 한다.
모든 가능한 분할 지점을 탐색하며 최적의 분할 위치를 찾는다.
## 2단계: 재귀적 해법
`m[i,j]` : A<sub>i</sub>..A<sub>j</sub>를 곱할 때 필요한 최소 곱셈 수
	•	i = j이면 `m[i,j] = 0`
	•	i < j이면 `m[i,j]` = min { `m[i,k]` + `m[k+1,j]` + p<sub>i-1</sub> × p<sub>k</sub> × p<sub>j</sub> } (i ≤ k < j)
`s[i,j]`: A<sub>i</sub>A<sub>i+1</sub>…A<sub>j</sub>를 최적 괄호 묶기로 나누기 위한 k의 값
## 3단계: 최적 비용 계산
재귀적으로 계산하는 대신, Bottom-up 방식의 테이블을 이용해 최적 비용을 계산한다.
입력: p = <p<sub>0</sub>, p<sub>1</sub>, …, p<sub>n</sub>>
보조 테이블:
	•	`m[1..n][1..n]`: `m[i,j]` 값 저장용
	•	`s[1..n][1..n]`: `s[i,j]` 분할 위치 저장용
![[Pasted image 20250521172950.png|300]]
![[Pasted image 20250521173000.png|300]]
`m[1][4]`를 계산하기 위해서 n-1개의 case가 있었음 
하나의 cell을 계산하기위해 O(log n) time이 걸렸고, 계산해야 하는 cell은 O(n<sup>2</sup>)이므로 총 O(n<sup>3</sup>) time에 수행된다.
![[Pasted image 20250521173008.png|300]]
**이건 했던대로 공부,, 중요한 파트**