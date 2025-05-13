## Model of Symmetric Cryptosystem
![[Pasted image 20250506170942.png|300]]
## 동기: 공개키 암호 시스템 (Public-Key Cryptosystems)
•	공개키 암호 방식의 개념은 대칭키 암호화(symmetric encryption)와 관련된 두 가지 가장 어려운 문제를 해결하려는 시도에서 발전함:
•	키 분배
	•	키 분배 센터(KDC)를 신뢰하지 않고도 일반적인 상황에서 안전한 통신을 하는 방법
•	디지털 서명
	•	메시지가 주장된 발신자로부터 온 것임을 확인하는 방법
•	스탠퍼드 대학교의 Diffie와 Hellman은 1976년에 이 두 문제를 해결하고 기존의 암호 방식들과는 전혀 다른 방식을 제시하여 큰 진전을 이룸

## 공개키 암호 시스템의 구성 요소
•	공개키 암호화는 다음의 여섯 가지 요소로 구성됨:
•	평문 (Plaintext): 알고리즘의 입력으로 들어가는 읽을 수 있는 메시지 또는 데이터
•	암호화 알고리즘: 평문에 변환을 수행
•	공개키: 암호화에 사용됨
•	개인키: 복호화에 사용됨
•	암호문 (Ciphertext): 출력으로 생성된 난독화된 메시지
•	복호화 알고리즘: 암호문과 개인키를 입력받아 원래의 평문을 생성
## Public Key Cryptosystem: Confidentiality
![[Pasted image 20250506171757.png|300]]
PU: public key로 모든 사람에게 나누어주는 키
PR: private key


## 전통적 암호화 vs 공개키 암호화

|          | 전통적 암호화                                                                                              | 공개키 암호화                                                                                                                 |
| -------- | ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| 동작<br>조건 | 1. 암호화와 복호화에 같은 알고리즘과 키 사용<br>2. 송신자와 수신자가 알고리즘과 키를 공유해야 함                                           | 1. 암호화와 복호화에 각각 연관된 다른 알고리즘 사용 <br>(쌍으로 된 키 중 하나씩 사용)<br>2. 송신자와 수신자는 서로 다른 키 한 쌍을 보유해야 함                               |
| 보안<br>조건 | 1. 키는 비밀로 유지되어야 함<br>2. 키가 비밀이면 메시지를 해독하는 것이 불가능하거나 비현실적이어야 함<br>3. 알고리즘과 암호문 샘플을 알아도 키를 추론할 수 없어야 함 | 1. 두 키 중 하나는 비밀로 유지되어야 함<br>2. 비밀 키가 유지되면 메시지를 해독하는 것이 불가능하거나 비현실적이어야 함<br>3. 알고리즘, 키 하나, 암호문 샘플을 알아도 다른 키를 유추할 수 없어야 함 |
## Public Key Cryptosystem: Authentication
![[Pasted image 20250506173844.png|300]]
보내는 사람은 자신의 private key를 가지고 서명
받는 사람은 보낸 사람의 public key를 가지고 서명 확인
서명은 자신만 할 수 있고, 서명 확인은 모두가 할 수 있음
MAC과 달리 부인 방지 기능이 추가된다 (서명 생성은 보내는 사람만 가능하므로)
그림 외우기
## Cryptosystem: Authentication and Confidentiality
![[Pasted image 20250506174659.png|300]]
1. 송신자의 private key로 암호화
2. 수신자의 public key로 암호화
3. 수신자의 private key로 복호화
4. 송신자의 public key로 복호화
송신자 수신자의 private, public key가 어떤 목적을 가지고 사용되는지 잘 알아야 한다.
=> 굉장히 중요한 개념
그림 외우기
## 공개키 암호 시스템의 응용 분야
•	세 가지 범주로 분류됨:
•	암호화/복호화: 송신자가 수신자의 공개키로 메시지를 암호화
•	디지털 서명: 송신자가 자신의 개인키로 메시지에 서명
•	키 교환: 양측이 협력하여 세션 키를 교환
•	일부 알고리즘은 세 가지 용도 모두에 적합하고, 일부는 한두 가지 용도에만 적합
![[Pasted image 20250506175832.png|300]]
## 공개키 암호 시스템의 요구사항
•	B 사용자가 공개키(Pub)와 개인키(Prv)를 생성하는 것이 계산적으로 쉬워야 함
•	A 사용자가 공개키와 메시지를 알고 있을 때 암호문을 생성하는 것이 쉬워야 함
•	B 사용자가 개인키로 암호문을 복호화하여 원문을 얻는 것이 쉬워야 함
=> 정당한 사용자는 계산이 쉬워야 한다
•	공격자가 공개키만 알고 있을 때 개인키를 알아내는 것이 계산적으로 불가능해야 함
•	공격자가 공개키와 암호문을 알고 있을 때 원문을 알아내는 것이 계산적으로 불가능해야 함
=> 정당하지 않은 사용자는 계산이 어려워야 한다.
## RSA 알고리즘
•	1977년 MIT의 Ron Rivest, Adi Shamir, Len Adleman이 개발
•	가장 널리 사용되는 범용 공개키 암호 방식
•	평문과 암호문은 0부터 n-1까지의 정수
•	일반적인 n의 크기는 1024비트(10진수로 약 309자리)
![[Pasted image 20250506180244.png|500]]
기억할 것:
	1.	Euler totient function 𝜙(𝑛)
	•	𝑛 이하의 양의 정수 중 𝑛과 서로소인 수의 개수
	2.	오일러 정리
	•	a와 n이 서로소일 때:
  a<sup>𝜙(n)</sup> ≡ 1 (mod n)
정당성 증명:
단순화를 위해, gcd(𝑀, 𝑛) = 1 인 경우만 고려한다.
𝑒𝑑 = 𝑘𝜙(𝑛) + 1 이 되는 어떤 𝑘가 존재한다고 하자.
𝐶<sup>𝑑</sup> mod 𝑛 = (𝑀<sup>𝑒</sup> mod 𝑛)<sup>𝑑</sup> mod 𝑛 => C = 𝑀<sup>𝑒</sup> mod 𝑛 이므로
      = 𝑀<sup>𝑒𝑑</sup> mod 𝑛 => mod 연산의 성질로 이렇게 표현 가능
      = 𝑀<sup>𝑘𝜙(𝑛)</sup>× 𝑀 mod 𝑛
      = (𝑀<sup>𝜙(𝑛)</sup>)<sup>𝑘</sup> × 𝑀 mod 𝑛 = 𝑀 => Euler totient function에 의해 𝑀<sup>𝜙(𝑛)</sup> = 1
=> M을 e 제곱해서 cipher text를 만들고, 이를 d 제곱을 다시하게 되면 원래 M이 돌아온다.
#### RSA Algorithm Example
![[Pasted image 20250506182828.png|400]]
p = 17
q = 11
𝜙(𝑛)  160
e = 7
d = 23

![[Pasted image 20250506184338.png|500]]
만약 mod 187에서 plain text가 189라면, decryption이후 값은 2가 되어버려 다른 값이 나올 수 있다
#### 해결 방법 두 가지
ECB mode처럼 plain text를 작은 숫자로 쪼개고, 첫 번째 block의 e제곱, 두 번째 block의 e제곱을 하고, 복호화 시에도 첫 번째 cipher text의 d제곱, 두 번째의 d제곱을 하면서 나중에 이를 다 합치는 방법을 사용한다.

하지만, 이는 너무 연산이 시간이 오래걸리게 되어 이렇게 잘 사용하지는 않는다. 
##### Hibrid encryption
Key encapsulation + Data encryption
encapsulated된 key인 C는 Alice만 복원할 수 있게 된다.
공개키 암호로 key를 encapsulation한 것과 Cipher text를 보내주고, 그 세션 key를 복원한 Alice가 실제 bulk 데이터에 대한 decryption을 할 수 있다.
## RSA encryption vs. digital signature
![[Pasted image 20250506185921.png|400]]
### RSA digital signature
![[Pasted image 20250506190031.png|400]]
메시지 M과 전자서명 S를 같이 보냄
Alice의 공개키 e,n을 가지고 M,S를 서명확인 할 수 있어 부인 방지가 가능하다
추가적으로 M과 M'이 같다는 것을 알면 무결성도 확인 가능하다.
## 모듈러 산술에서의 지수 계산
•	RSA에서 암호화, 복호화, 서명 생성 및 검증은 모두 정수를 지수 제곱한 후 mod n을 취함
•	모듈러 연산의 성질 사용 가능: (a mod n) × (b mod n)] mod n = (a × b) mod n
•	지수가 매우 크기 때문에 지수 연산의 효율성 중요
### Algorithm for Computing a<sup>b</sup> mod n (Binary modular exponentiation / Square and Multiply method)
![[Pasted image 20250506190719.png|400]]
a = 7
b = 560
n = 561
이고, 560은 10bit로 표현되며 따라서 k = 9가 된다.
c는 특정 비트까지의 값을 10진수로 표현한것, ex) i가 4일 때 100011이며, 이는 십진수로 35로 표현됨
7<sup>1</sup>을 이용해 7<sup>10</sup>을 만들고 이를 이용해 7<sup>1000110000</sup>을 만드는 방법임
복잡도 -> 비트 자리 수 정도만 하므로 k bit라면, k x 3/2 정도의 곱셈을 하게 된다.

## 공개키로의 효율적 연산
•	RSA에서 공개키 사용 시 속도를 높이기 위해 e 값은 보통 특정 값으로 설정
•	가장 일반적인 값: 65537 (2¹⁶ + 1)
	•	다른 값들: e = 3, e = 17
	•	이 값들은 1 비트가 적어 곱셈 횟수가 최소화됨
	•	단, e = 3처럼 너무 작은 e는 간단한 공격에 취약
## 개인키로의 효율적 연산
•	복호화는 d 제곱 연산 포함
	•	d가 작으면 무차별 대입 공격 등 다양한 공격에 취약 (그래서 d는 작게 하면 안됨)
•	계산 속도를 높이기 위해 중국인의 나머지 정리(CRT) 사용 가능
	•	d mod (p-1), d mod (q-1)을 미리 계산
	•	직접 계산보다 약 4배 빠르게 처리 가능
## RSA의 보안
•	RSA에 대한 공격 방식 5가지:
	•	Brute force: 가능한 모든 개인키 시도
	•	Mathematical attacks: 두 소수의 곱을 인수분해
	•	Chosen ciphertext attacks (CCA): RSA의 동형성 속성 악용
	•	Implementation attacks: 
	•	side-channel
		예: 시간 분석, 전력 분석
	•	Hardware fault-based attack
		•	복호화 또는 서명 중인 프로세서에 인위적 오류 유도
## Factoring Problem
![[Pasted image 20250508153122.png|300]]
•	RSA 공격을 위한 수학적 접근 3가지:
	•	n을 두 소수로 인수분해 → φ(n) = (p-1)(q-1) → d 계산
	•	p, q 없이 φ(n) 직접 계산 → d 계산 가능
	•	φ(n) 없이 d를 직접 계산
## Chosen Ciphertext Attack (CCA) 
•	공격자는 여러 개의 암호문을 선택하고, 해당 암호문을 타겟의 개인 키로 복호화한 평문을 제공받는다.
	•	즉, 공격자는 임의의 평문을 선택해 타겟의 공개 키로 암호화하고, 이를 다시 개인 키로 복호화함으로써 평문을 얻을 수 있다(복호화 오라클에 대한 질의).
	•	공격자는 RSA의 특성을 악용해, 타겟의 개인 키로 처리했을 때 암호 해독에 필요한 정보를 얻을 수 있는 데이터 블록을 선택한다.
	•	간단한 예:
		▪ 공격자의 목표는 개인 키 d 없이 암호문 C를 복호화하는 것이다.
		▪ 공격자는 C' = C X 2<sup>e</sup> mod n을 계산한 후, 복호화 오라클에 C'을 복호화해달라고 요청해 M' = C'<sup>d</sup> \mod n을 얻는다.
		▪ 그런 다음, 공격자는 M' X 2<sup>-1</sup> mod n을 계산한다.
	•	이러한 공격을 방지하기 위해, RSA Security Inc.는 평문을 OAEP(최적 비대칭 암호화 패딩)이라는 절차를 이용해 수정할 것을 권장한다.
## Encryption Using Optimal Asymmetric Encryption Padding (OAEP)
![[Pasted image 20250508153448.png|400]]
C = (EM)<sup>e</sup> mod n
## side-channel Attacks
![[Pasted image 20250508153623.png|400]]
•	암호 전문가 Paul Kocher가 개발
•	RSA뿐 아니라 다른 공개키 방식에도 적용됨
•	특징:
	•	예상하지 못한 방식으로 이루어짐
	•	암호문만으로 공격 가능
•	대응책:
	•	키에 독립적인 연산 (예: 일정한 시간의 지수 계산)
	•	무작위성 도입 (예: 무작위 지연)
## Fault-Based Attack
•	RSA 서명을 생성 중인 프로세서를 공격
	•	전력을 줄여 계산 중 오류를 유도
	•	잘못된 서명 결과를 분석해 개인키 유추
•	알고리즘은 단일 비트 오류를 유도하고 결과를 관찰함

![[Pasted image 20250513160207.png|300]]
### Chinese Remainder Theorem
- chinese Remainder Theorem을 이용해 연산의 복잡도를 4배까지 줄일 수 있다.

- 과정
	- 비밀 키를 d<sub>p</sub> = d mod (p-1), d<sub>q</sub> = m<sup>d<sub>q</sub></sup> mod q로 나눈다.
	- 각각의 서명을 아래와 같이 계산할 수 있다
		- s<sub>p</sub> = m<sup>d<sub>q</sub></sup> mod p, s<sub>q</sub> = m<sup>d<sub>q</sub></sup> mod q
	- 전체 서명은 CRT를 이용해 결합한다
		- s = as<sub>p</sub> + bs<sub>q</sub> mod n
- 어떤 N 계산시 N = mod p, mod q로 쪼갤 수 있다.
	- 공격자가 한쪽만 오류를 발생시키면 p, q 중 하나를 알아낼 수 있다. (N은 알려)




## 공개키 암호에 대한 오해
•	공개키 암호는 대칭키 암호보다 항상 더 안전하다
•	공개키 암호와 대칭키 암호는 키 길이가 같으면 보안 수준도 같다
	128bit AES는 brute force를 써야하는데, 2048 bit RSA는 2<sup>112</sup>복잡도로 깨질 수 있음 (Factoring으로)
•	공개키 암호는 범용 기술로 대칭키 암호를 대체했다
•	공개키 암호는 키 분배가 간단해서 대칭키 방식에서 필요했던 복잡한 키 교환 절차가 불필요하다
	RSA는 상대의 공개키가 정말 상대의 공개 키인지 모르기 때문에 복잡한 키 교환 절차가 필요할 수 있다.