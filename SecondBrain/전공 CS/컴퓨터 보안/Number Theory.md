## Divisibility
### 정의
0이 아닌 정수 b가 a를 나눌 수 있다면 `b | a`라 한다.
a = mb를 만족한다
즉, a % b가 0이다.
### example
- `-5 | 30`
	- m = -6
- `17 | 0` 
	- m이 0이라면, 0 = 17 x 0 이므로 `17 | 0`이 성립한다.
- a | 1 이면 a=±1
- a | b and b | a then a = ±b
- b가 아닌 모든 정수는 0을 나눈다
- a | b and b | c면 a | c
- b | g and b | h면 b | (mg + nh)
## Division Algorithm
a = qn + r일 때, r은 항상 0보다 커야 한다.
### example
-40 = -2 x 15 - 10 (X)
-40 = -3 x 15 + 5 (O)
## Euclidean Algorithm
### Greatest Common Divisor (GCD)
**gcd(0,0)=0으로 정의**한다.
일반적으로, gcd(a,b) = gcd( `| a |` , `| b |` )
#### example
gcd(0,0) = 0
gcd(a,0) = |a|
a,b가 서로소 일때, gcd(a,b) = 1
### Euclidean Algorithm
![[Pasted image 20250317164250.png|250]]
![[Pasted image 20250317164418.png|100]]
a, b, r이 있을 때(a > b),
r = a % b
r이 0이면, b가 최대공약수
아니라면, a에 b 대입, b에 r 대입하고 다시
## Modular 연산
a mod n에서 n(modulus)은 양수여야 한다.
### example
11 mod 7 = 4
-11 mod 7 = 3
### Congruene (동치)
두 정수 a와 b가 modulo n에서 congruent하다
=> `a mod n = b mod n`
=> `a ≡ b (mod n)`
a ≡ 0 (mod n)이면, `n | a` 다.
#### 동치 관계의 성질
1. a ≡ b (mod n) ⇔ n | (a - b)
	1. a-b = kn이라 하면,
	2. a mod n = (b + kn) mod n = b mod n
2. a ≡ b (mod n)이면 b ≡ a (mod n) (대칭성)
3. a ≡ b (mod n)이고 b ≡ c (mod n)이면 a ≡ c (mod n) (추이성)
#### 모듈러 연산의 성질
1. \[(a mod n) + (b mod n)] mod n = (a + b) mod n
2. \[(a mod n) - (b mod n)] mod n = (a - b) mod n
3. \[(a mod n) * (b mod n)] mod n = (a * b) mod n
### Arithmetic Modulo 8
![[Pasted image 20250317165956.png|200]]
### Multiplication Modulo 8
modular 연산에 따라 값의 관계가 바뀐다.
![[Pasted image 20250317170036.png|200]]
### Additive and Multiplicative Inverse Modulo 8
곱셈의 역원
![[Pasted image 20250317170158.png|150]]
두 번째 열: w + (-w) ≡ 0 mod 8을 만족하는 수
세 번째 열: w x w<sup>-1</sup> ≡ 1 mod 8을 만족하는 수
8과 1은 서로소이기 때문에 곱셈의 역원이 있다.
따라서 소수는 곱셈의 역원이 항상 있다,
x \* □ mod 7 = 1 이면, 
x에는 1~6이 들어갈 수 있다.
x:1, x<sup>-1</sup>:8
x:2, x<sup>-1</sup>:4
x:3, x<sup>-1</sup>:5
### 모듈러 연산의 속성
#### 교환 법칙
`(w + x) mod n = (x + w) mod n`
`(w × x) mod n = (x × w) mod n`
#### 결합 법칙
`[(w + x) + y] mod n = [w + (x + y)] mod n`
`[(w × x) × y] mod n = [w × (x × y)] mod n`
#### 분배 법칙
`[w × (x + y)] mod n = [(w × x) + (w × y)] mod n`
#### 항등원
`(0 + w) mod n = w mod n`
`(1 × w) mod n = w mod n`
#### 덧셈의 역원
모든 w ∈ Z<sub>n</sub>에 대해, 어떤 z가 존재하여 w + z ≡ 0 (mod n)을 만족함.
### 확장 유클리드 알고리즘
곱셈의 역원을 찾는 알고리즘
곱셈의 역원은 GCD(a,b) = 1이어야 곱셈의 역원을 찾을 수 있다.
주어진 두 정수 a, b에 대해 확장 유클리드 알고리즘은 정수 x, y를 찾아 `ax + by = gcd(a, b)`를 만족하는 해를 구한다.
일반적인 유클리드 알고리즘은 gcd(a, b)만 구하지만, 확장된 버전에서는 추가로 x와 y값을 찾는다.
#### example
GCD(1759, 550) = 1일 때, modulo 1759에서 550의 곱셈의 역원을 찾는 문제
![[Pasted image 20250318224223.png|300]]
![[알고리즘 풀이-1.jpg|500]]
## Fermat's Theorem
p가 소수고, a가 p로 나뉘어지지 않는 양수라면 아래 식을 만족한다
a<sup>p−1</sup> ≡ 1 (mod p)
a<sup>p</sup> ≡ a (mod p)
### Fermat test
n이 소수인지 판별하려면, random한 a에 대해 a<sup>n-1</sup>mod n != 1인 값을 찾아야 한다. ( a ∈ \[1, n-1] )
페르마의 소정리에 따라, n가 소수라면, 어떤 a를 선택하든 a<sup>n−1</sup>를 n으로 나눈 나머지는 항상 1이 되어야 하기 때문이다.

Repeat
	choose random a ∈ \[1, n-1]
	compute y=a<sup>n−1</sup> mod n
	if y != 1 return "composite"

## Some Values of Euler's Totient Function
![[Pasted image 20250320104053.png|400]]
Euler's Totient Function ( ϕ(n) ): n을 입력으로 주면, n보다 작은 것 중 n과 서로소인 것의 개수를 주는 함수 (대신, ϕ(1) = 1)
n이 prime이면, prime-1개가 나온다.
## Euler's Theorem
![[Pasted image 20250320104347.png|100]]
이것만 기억하기
n이 소수일 때, ϕ(n) = n-1이므로 이가 성립한다.
## Primality Test
- Trial division
	- 에라토스테네스의 체같은 방식 : 2부터 루트 n까지의 소수로 나누어 떨어지는지 확인한다
	- 틀리지 않는 알고리즘이지만 느리고, 나머지는 답이 틀릴 수도 있는 알고리즘이다.
- Fermat test
- Miller-Rabin test
	- Fermat test를 기반으로 하지만, 추가적인 검사를 수행함
	- NSR
- Hybrid
	- ex) trial division + Miller-Rabin/Fermat
	- 작은 수만 trial division으로 소수 판별을 하고, 
	- 일정 지점을 넘어가면 Miller-Rabin/Fermat을 이용해서 소수를 찾는다.
## Deterministic Primality Algorithm
소수 판별 알고리즘은 자리수에 따라 복잡도가 결정된다. 
Trial division은 지수 알고리즘, AKS는 자리수의 6제곱인데, 너무 오래 걸려서 보통 Miller-Rabin 또는 Fermat, Hybrid를 사용한다 (자리수의 제곱정도로 작동함)
## Powers of Integers, Modulo 19
![[Pasted image 20250320110017.png|300]]
Discreate Log(이산 대수): 
a<sup>x</sup> mod p = y일 때, a, p, y를 주고 x를 찾아야함
a,p,y에 대한 이산 대수가 x이다.
# 모듈러 거듭제곱과 이산 로그에 대한 몇 가지 속성과 정의

- $ p = 19 $ 는 소수이다. 우리는 임의의 $ a < p $ 에 대해  
  $$ a^{p-1} \equiv 1 \pmod{p} $$ 임을 안다.  
  모든 수열은 1에서 끝나며, 여기서 $ p - 1 = 18 $ 이다.  

- $$ a^m \equiv 1 \pmod{p} $$ 을 만족하는 최소한의 $ m $ 은 항상 $ p - 1 = 18 $ 이 아니다.  
  우리는 이 $ m $ 을 **위수(order)** 라고 부른다.  
- $ m $ 은 $ p - 1 $ 을 나눈다.  
  즉, 각 행에서 반복되는 수열의 개수는 정수이다.  
- 어떤 $ a $ 의 위수가 $ p - 1 $ 일 수도 있다.  
  이러한 $ a $ 는 19를 법으로 하는 0이 아닌 정수들의 집합을 생성한다.  
  이러한 정수를 **원시 근(primitive root)** 또는 **생성자(generator)** 라고 한다.  
- 새로운 문제를 정의할 수 있다:  
  주어진 소수 $ p $ 에 대해, 정수 $ y $ 와 원시 근 $ a $ 가 주어졌을 때,  
  유일한 지수 $ x $ 를 찾아야 한다.  
  즉,  

  $$ a^x \equiv y \pmod{p} $$  

  를 만족하는 $$ 0 \leq x \leq p - 1 $$ 인 $ x $ 를 찾는 문제이다.  
  이때, **유일한 지수 $ x $ 를 $ y $ 의 이산 로그(discrete logarithm)** 라고 한다.  
  이 문제를 **이산 로그 문제(discrete logarithm problem, DLP)** 라고 부른다.  

- **표기법:**  
  $$ x = \log_{a,p} y $$

## Tables of Discrete Logarithms, Modulo 19

![[Pasted image 20250320110100.png|300]]