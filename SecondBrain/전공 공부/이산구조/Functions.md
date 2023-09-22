# Functions

A의 각 원소가 B의 각 하나의 원소만을 대응해야한다.

f(a) = b 이면,
b는 f 안에서 a의 image라고 불린다.
a는 f 안에서 b의 preimage라고 불린다.

## 예제

![[Pasted image 20230920201239.png]]
f(A) = {y,z}
y의 preimage는 b
f의 codomain은 B
f의 domain은 A
d의 image는 z
z의 preimage는 {a,c,d}
f{a,c,d} = {z}
f{a,b} = {y,z}

## Injections(단사 함수)
f(x) -> f(y)
같은 이미지가 있으면 안된다.
![[Pasted image 20230922120403.png]]
## Surjections (전사 함수)
정의역이 치역을 전부 사용하는 관계
![[Pasted image 20230922120351.png]]
# Bijections (일대일 대응함수, 양사 함수)
one-to-one의 관계

Bijections이려면 , Injections여야 하며, Surjections여야 한다.
![[Pasted image 20230922120415.png]]
## Inverse Functions (역함수)
f를A->B인 bijection이라고 할 때 B->A인 함수

## Composition (합성 함수)
f : B->C, g : A->B 일 때, fㆍg = f(g(x))

## floor/ceiling function
x보다 같거나 작은 수 중 가장 큰 정수
⌊x⌋ ex) ⌊3.5⌋ = 3
x보다 작거나 같은 수 중 가장 작은 정수
⌈x⌉ ex ) ⌈-1.5⌉ = -1

## Factorial Function
f: N -> Z⁺ , f(n) = n!
f(0) = 0! = 1
