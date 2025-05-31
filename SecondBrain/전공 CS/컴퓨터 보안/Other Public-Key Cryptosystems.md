## 10장: 목차
•	복습: 공개키 암호 시스템의 응용
•	복습: 이산 로그
1.	디피-헬만 키 교환
	•	키 교환 프로토콜 => Discrete log 사용
	•	Forward Secrecy => DH는 Forward Secrecy를 만족함
	•	man-in-the-middle attack => RSA, DH의 문제점
2.	Elgamal crpyographic system => Discrete log 사용
3.	타원 곡선 암호 => Discrete log와 구조상 비슷
	•	디피-헬만 키 교환의 유사체
	•	타원 곡선 암호의 보안성 => 같은 key 길이로 높은 안정성을 부여한다.
## 복습: 공개키 암호 시스템의 응용
•	공개키 암호 시스템은 세 가지 범주로 분류될 수 있다:

•	암호화/복호화
	▪ 송신자는 수신자의 공개키로 메시지를 암호화한다
•	디지털 서명
	▪ 송신자는 자신의 개인키로 메시지에 “서명”한다
•	키 교환
	▪ 양측이 협력하여 세션 키를 교환한다
## Mod 19에서 Discrete logarithm
![[Pasted image 20250513163056.png|300]]
## 디피-헬만 키 교환
•	*최초로 발표된 공개키 알고리즘*
•	여러 상용 제품들이 이 키 교환 기법을 사용
•	두 사용자가 보안을 유지한 채 키를 교환할 수 있도록 해주며, 이 키는 이후 대칭 암호 방식으로 메시지를 암호화하는 데 사용됨
•	알고리즘 자체는 비밀 값의 교환에만 제한됨
•	그 효과는 이산 로그 계산의 어려움에 의존함
## 도표 10.1 디피–헬만 키 교환
![[Pasted image 20250513163221.png|500]]
X<sub>A</sub>: Alice만 앎
X<sub>B</sub>: Bob만 앎
나머지 값들은 모두 아는 값들임

송신자는 X<sub>A</sub>를 가지고, Y<sub>A</sub> = a<sup>X<sub>A</sub></sup> mod q를 수신자에게 보냄
수신자도 X<sub>B</sub>를 가지고, Y<sub>B</sub> = a<sup>X<sub>B</sub></sup> mod q를 송신자에게 보냄
K = (Y<sub>B</sub>)<sup>X<sub>A</sub></sup> mod q = (Y<sub>A</sub>)<sup>X<sub>B</sub></sup> mod q임
## RSA vs. DH: Forward Secrecy
Forward Secrecy: 앞쪽은 안전하다!
RSA의 경우, 영구적인 개인 키를 사용하기 때문에, cipher text 값을 logging해서 가지고 있다면, 2에서 뚫려도 1도 뚫릴 수 있다.
따라서 이를 사용하는 이전 모든 세션이 뚫릴 수 있다.
하지만, *DH의 경우, Forward Secrecy를 만족한다*. (X<sub>A1</sub>, X<sub>B1</sub>을 한 세션만 쓰고 사용 후 버리기 때문)
![[Pasted image 20250515154150.png|400]]
## Man-in-the Middle Attack
![[Pasted image 20250515154243.png|200]]
원래라면, Alice와 Bob은 서로 같은 Key를 가져야 하는데, Darth가 중간에 각각 Y<sub>A</sub>와 Y<sub>B</sub>를 가로채 K1과 K2를 만들어 Alice, Bob과 통신하게 된다. Alice는 Bob도 K2를 알 것이라 생각하고, Bob은 Alice도 K1을 알 것이라 생각하기 때문에 서로 안전하게 통신하고 있다고 착각할 수 있다.
=> 이 문제는 공개키 인증서가 이를 해결해줄 수 있다.
## The ElGamal Cryptosystem
•	1984년 T. Elgamal이 발표
•	이산 로그 문제에 기반한 공개키 방식
•	디피-헬만과 밀접한 관련 있음
- k가 random integer이기 때문에, 똑같은 M을 암호화해도 다를 수 있다.
![[Pasted image 20250515154630.png|300]]
###### ElGamal Cryptosystem의 전역 공개 요소
q : 큰 소수
a: q의 primitive root
###### ElGamal Cryptosystem의 key generation
Y<sub>A</sub> = α<sup>X<sub>A</sub></sup> mod q
PU: {q, α, Y<sub>A</sub>}
PR: X<sub>A</sub>
###### ElGamal Cryptosystem의 Encryption 과정
세션 키 K 계산: K = (Y<sub>A</sub>)<sup>k</sup> mod q
C1 (세션 키 계산 용) 계산 : α<sup>k</sup> mod q
C2 (메시지 계산 용) 계산: KM mod q
###### ElGamal Cryptosystem의 Decryption 과정
K Decryption: K = C<sub>1</sub><sup>X<sub>A</sub></sup> mod q
M Decryption: M = C<sub>2</sub>K<sup>-1</sup> mod q

## Elliptic Curve Arithmetic (타원 곡선 연산)
•	암호화 및 디지털 서명을 위해 공개키 암호를 사용하는 대부분의 제품 및 표준은 RSA를 사용
	•	안전한 RSA 사용을 위한 키 길이는 최근 몇 년 사이 증가하였고, 이는 RSA를 사용하는 애플리케이션에 더 많은 처리 부하를 유발함
•	타원 곡선 암호(ECC)는 다음과 같은 표준화 노력에서 등장하고 있음:
	•	IEEE P1363 공개키 암호화 표준
	•	IEEE 1609 차량 환경에서의 무선 접속 표준(WAVE)
•	*RSA/DH보다 훨씬 짧은 키 길이로 동일한 수준의 보안을 제공*
## 비교 가능한 보안 강도
•	NIST 특별 간행물 800-57 1부 개정 5, 2020년 5월 (키 관리 권고사항: 1부 – 일반)
![[Pasted image 20250515155233.png|200]]
security Strengh: 공격 시 O(2<sup>x</sup>) 연산이 필요하다는 의미
결론: 타원 곡선은 더 짧은 key를 써도 안전하게 가능하다
ECC는 Security Strength가 O(root(f))이다.
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
## Example of Elliptic Curves
![[Pasted image 20250515155515.png|200]]

- ECC에서의 덧셈 연산은 RSA에서의 모듈러 곱셈에 해당
- 다중 덧셈(= 스칼라 곱셈, 점 곱셈)은 RSA에서의 모듈러 거듭제곱에 해당
- ECDLP
	- Q = kP, 여기서 Q, P는 타원 곡선 상의 점
	-  주어진 k와 P로 Q를 계산하는 것은 쉬움 (예: 이진법 방법)
```
Q ← O.
for i ← l to 0  // k = kl kl−1 … k0
	Q ← 2Q.
	if ki = 1 then Q ← Q + P.
return Q.
```
- 하지만 주어진 Q와 P로부터 k를 찾는 것은 어려움
- 타원 곡선 이산 로그 문제(ECDLP)라고 알려짐
ECDLP: 곡선 E, P, Q를 죽 Q = kp를 만족하는 k를 찾는 것
discrete log: α, q, y가 주어졌을 때, x를 찾는 것 (같은 구조라고 한다)
## ECC Diffie-Hellman Key Exchange
![[Pasted image 20250515155900.png|300]]
###### ECC DH Key Exchange 전역 공개 요소
E<sub>q</sub>(a,b) : 타원 곡선
G: 타원 곡선 위의 점
###### ECC DH Key Exchange key generation
User A
PU: P<sub>A</sub> = n<sub>A</sub> x G
PR: n<sub>A</sub> = n보다 작은 임의의 정수

User B
PU: P<sub>B</sub> = n<sub>B</sub> x G
PR: n<sub>B</sub> = n보다 작은 임의의 정수
###### ECC DH Key Exchange 과정
K = n<sub>A</sub> x P<sub>B</sub>
K = n<sub>B</sub> x P<sub>A</sub>
![[Pasted image 20250515155918.png|200]]
## 타원 곡선 암호의 보안성
•	ECDLP의 어려움에 의존
•	알려진 가장 빠른 기법은 “Pollard rho method”
•	소인수 분해(및 DLP)에 비해 훨씬 작은 키 크기를 사용할 수 있음
•	동등한 키 길이에서는 연산량이 대체로 비슷함
•	따라서 유사한 보안 수준에서 ECC는 상당한 계산상의 이점을 제공함
