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
gcd(0,0)=0으로 정의한다.
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
## Prime Numbers
여기까지