# Sets
순서가 중요하지 않음. ex) {a,b,c,d} = {b,c,d,a} = {a,b,c,d,b,c}
중복이 의미가 없음

## Some Important Sets
N = 자연수 natural
Z = 정수 zungsu
Z⁺ = 양의 정수
R = 실수 real
R⁺ = 양의 실수
C = 복소수
Q = 유리수

## 구간 표기법
\[a,b] = {x | a≤x≤b} : 폐구간
\[a,b) = {x | a≤x<b}
(a,b] = {x | a<x≤b}
(a,b) = {x | a<x<b} : 개구간

∅ ≠ {∅}

집합은 집합의 원소가 될 수 있다.

## 집합의 동등성
`∀x( x∈A ↔ x∈B )` ≡ `∀x[ (x∈A → x∈B) ∧ (x∈B → x∈A) ]` ≡ `A⊆B and B⊆A`

# **부분집합 증명**
![[Pasted image 20231015203106.png]]
# **진부분집합 증명**
![[Pasted image 20231015203156.png]]

## 진 부분집합
A⊆B이지만, A≠B인 집합

## Set Cardinality (집합의 크기)
\| {a,b,c} | = 3 

> 공집합은 원소가 아니다!
## **Power Sets**
집합 A의 모든 subset, `P(A)`라고도 부른다.
n개의 원소가 있을 때, power set의 cardinality는 2ⁿ개이다. 
P(A) = { ∅ , {a} , {b} , {a,b} }

## Tuples
orderd n-tuple은 순서가 있는 집합이다.
2-tuple 은 ordered pairs라고 불린다.
ordered pairs인 (a,b)와 (c,d)가 같다면, a=c , b=d이다.

## **Cartesian Product**
A = {a,b} B = {a,b,c} 일 때,
AxB = {{a,1}, {a,2}, {a,3}, {a,4}, {b,1}, {b,2}, {b,3}, {b,4} }

## 한정자의 Truth Sets
P와 도메인 D가 주어질 때, truth set을 정의할 수 있다.
`{x∈D | P(x)}`
domain(정의역)이 정수이고, P(x)가 |x|=1이면 set은 {-1,1}이다.

## Complement
Ā = U - A 

## Difference
![[Pasted image 20231015203615.png]]

## 드모르간 법칙

![[Pasted image 20230920200251.png]]

## 멤버십 테이블

![[Pasted image 20230920200410.png]]

