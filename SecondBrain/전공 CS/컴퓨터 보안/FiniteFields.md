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
GF(15)는 있을 수 없음
## Finite Fields of the Form GF(p)
Finite Fields는 많은 암호 알고리즘에서 중요한 역할을 한다.
Finite Fields의 크기는 항상 소수 p의 거듭제곱 형태를 가져야 한다.
크기가 p<sup>n</sup>인 Finite Fields는 일반적으로 GF(p<sup>n</sup>)으로 표기된다.
## Arithmetic Modulo 8 and Modulo 7
### Addition modulo 8 vs 7
![[Pasted image 20250401141406.png|200]]
![[Pasted image 20250401141731.png|200]]
### Multiplication modulo 8 vs 7
![[Pasted image 20250401141443.png|200]]
곱셈의 역원이 항상 존재하지는 않는다.
![[Pasted image 20250401141627.png|200]]
0을 제외한 모든 원소가 곱셈의 역원을 가지기 때문에 모든 행에 1이 있다.
### Additive and multiplicative inverses modulo 8 vs 7
![[Pasted image 20250401141533.png|200]]
![[Pasted image 20250401141753.png|200]]
0을 제외한 모든 원소가 곱셈의 역원을 가진다.
## GF(p)의 성질
GF(p)는 p개의 원소로 구성된다. (0~p-1)
집합 내에서 덧셈, 곱셈, 나눗셈 연산을 수행할 수 있고, 0을 제외한 모든 원소는 곱셈에 대한 역원을 가짐
## 다항식 나눗셈
x<sup>2</sup>-1 = (x+1)(x-1)이 되지만, x<sup>2</sup>+x+1은 분해가 되지 않으므로 prime polynomial이라 한다.
## GF(2)에서 Polynmial Arithmetic
![[Pasted image 20250401142319.png|300]]
Addition에서 mod 2를 해서 없어지는 모습을 볼 수 있다.
![[Pasted image 20250401142329.png|300]]
GF(2)에서는, x<sup>2</sup>+1이 prime polynomial이 아니다.
x<sup>2</sup>+1 = x<sup>2</sup>+2x+1 이고, (x+1)<sup>2</sup>이기 때문이다.
## Polynomial GCD
기존 GCD와 같이 a(x)와 b(x)를 동시에 나누는 다항식 중 차수가 가장 큰 것을 의미한다.
#### example
GCD(x<sup>2</sup>+x , x<sup>2</sup>+1) = x+1
## Extended Euclid
![[Pasted image 20250401142818.png|400]]
## Polynomial Arithmetic Modulo (x<sup>3</sup> + x + 1)
### Addition
![[Pasted image 20250401142940.png|400]]
### Multiplication
![[Pasted image 20250401143029.png|400]]
## Arithmetic in GF(2<sup>3</sup>)
GF(2<sup>3</sup>)은 체이므로 항상 곱셈의 역원이 존재하고, 덧셈했을 때, 0이 되는 값이 존재한다.
### Addition
덧셈 연산에 대해 Abelian Group을 형성
![[Pasted image 20250401143106.png|300]]
### Multiplication
항상 곱셈의 역원 존재
![[Pasted image 20250401143139.png|300]]
### Additive and multiplicative inverses
![[Pasted image 20250401143156.png|100]]
## Computational Considerations
계수들이 0,1이기 때문에 이를 비트 문자열로 표현할 수 있다.
- 덧셈: XOR 연산
- 곱셈: shift와 XOR
모듈로 나눗셈에 의한 나머지 계산은 최고차 항을 약수 다항식으로 나눈 나머지로 반복적으로 대체함으로서 수행된다. (이것도 shift와 xor로 처리됨)
#### example
![[Pasted image 20250401143248.png|300]]
x<sup>n</sup> mod (x<sup>n</sup>+(something)) = something