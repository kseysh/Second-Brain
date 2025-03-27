# Groups
이항 연산 (•)을 갖는 원소들의 집합
Group은 아래 5가지를 만족해야 함 ( a ∈ G )
### Closure (폐쇄성)
a, b가 G에 속하면 a•b도 G에 속해야 한다.
### Associativity (결합법칙)
a • (b • c) = (a • b) • c
### Identity Element (항등원)
G에는 항등원 e가 존재하여,  a•e = e•a = a가 성립해야 한다.
### Inverse Element (역원)
G내에 역원 a<sup>-1</sup>이 존재하여 a•a<sup>-1</sup> = a<sup>-1</sup>•a = e가 성립해야 한다.
### Commutativity (교환법칙) -> Abelian Group (아벨 군)
a•b = b•a (이를 아벨 군이라고 한다.)
# Cyclic Group
G의 모든 원소가 어떤 고정된 원소 a ∈ G 의 거듭제곱 형태 a<sup>k</sup>(=a•a•a•a•a...)로 표현될 수 있음을 의미
이러한 원소 a를 G의 생성자라고 하며, G를 a에 의해 생성된 군이라고 한다.
Cyclic Group은 항상 Abelian Group이며 유한할 수 있고 무한할 수도 있다.
항등원은 a<sup>0</sup>=e로 정의된다.
#### example
![[Pasted image 20250327143017.png|300]]
# Fields
덧셈과 곱셈이라는 이항 연산을 가지는 원소들의 집합
a, b, c ∈ F 에 대해 아래 공리를 만족해야 한다.
### Additive Abelian Group
F는 덧셈 연산에 대해 Abelian Group을 형성한다
### Multiplicative Abelian Group (0 제외)
위 다섯 가지 성질이 곱셈 연산에서도 성립하며, 이는 곱셈에 대한 아벨군을 의미한다.
#### 곱셈의 역원
F의 모든 원소 a에 대해, a<sup>-1</sup> ∈ F 가 존재하여 
a•a<sup>-1</sup> = a<sup>-1</sup>•a = 1
을 만족해야 한다.

나눗셈은 다음 규칙으로 정의된다.
a/b = a•b<sup>-1</sup>
### 0 인수분해 금지
만약, a, b ∈ F 이고, a • b = 0이라면, 
a = 0 또는 b = 0 중 적어도 하나는 0이어야 한다.
### 분배 법칙
a • (b + c) = a • b + a • c
(a + b) • c = a • c + b • c
#### example
유리수, 실수, 복소수는 Fields의 대표적인 예시이다.
하지만, 정수 집합은 Fields가 아니다. (모든 원소가 곱셈의 역원을 가지지 않기 때문 2의 역원은 정수가 아님)
## Types of Fields
![[Pasted image 20250327144107.png|300]]
## Finite Fields of the Form GF(p)
Finite Fields는 많은 암호 알고리즘에서 중요한 역할을 한다.
Finite Fields의 크기는 항상 소수 p의 거듭제곱 형태를 가져야 한다.
크기가 p<sup>n</sup>인 Finite Fields는 일반적으로 GF(p<sup>n</sup>)으로 표기된다.