# Mathematical Induction (수학적 귀납법)

## 수학적 귀납법의 원리
1. Basis step ( P(1) )이 true라는 것을 증명해야 한다.
2. 귀납 단계 : p(k)가 true라고 가정할 때, p(k+1)이 true면 증명이 된다.

![[Pasted image 20231005093017.png]]

ex)
![[Pasted image 20231005093052.png]]
 P(1)이 true이다 라고만 쓰면 안되고, 직접 해서 증명해주어야 한다.
 p(k)가 true라고 가정할 때, p(k+1)이 true이면 모든 k에 대해서 true이다.
 
 ![[Pasted image 20231005093833.png]]

### Proving Inequalities
![[Pasted image 20231005094041.png]]

![[Pasted image 20231005094114.png]]
4로 시작하므로 p(4)가 true라고 가정해야 함 (항상 P(1)에서 시작하지는 않음)

### Proving Divisibility Results 
![[Pasted image 20231005094156.png]]

Number of subsets of a finite set은 안나옴

# Recursive Definitions and Structural Induction

## Recursively Defined Functions
Basis step : f(0)의 값을 구한다.
Recursive Step : 더 작은 정수의 값에서 정수의 값을 찾는 규칙을 지정한한다.

![[Pasted image 20231005095619.png]]
## 집합을 수학적 귀납법으로 표현
basis step이 어떻게 되어 있는지 초기 상태를 표현한다.
recursive step을 만든다.
새로운 elements를 만들어서 기존의 set에 추가하여 집합을 키운다.

![[Pasted image 20231005100210.png]]
Subset of Integers S: {3|s ∧ s ∈ z<sup>+</sup>}

## Strings
정의
![[Pasted image 20231012222911.png]]

만약, Σ = {a,b}이면, aab가 Σ\*안에 있는 것을 증명하라

λ ∈ Σ* 이기 때문에 a ∈ Σ, a∈ Σ*
a ∈ Σ* 이기 때문에 a ∈ Σ, aa∈ Σ*
aa ∈ Σ* 이기 때문에 b ∈ Σ, aab ∈ Σ*
이다.

