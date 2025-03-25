# Contents
- Symmetric Cipher Model
	- Terminology
- Substitution techniques
	- Monoalphabetic ciphers
		- Caesar cipher
		- Permutation cipher
	- Multiple-letter ciphers
		- Playfair cipher
	- Polyalphabetic ciphers
		- Vigenere
		- Autokey
		- Vernam
		- One-time pad
- Transposition techniques
- Rotor machines
- Steganography
## 용어 정의
Plaintext(평문) : original message
Ciphertext : 암호화된 message
Enciphering/encryption: plaintext에서 ciphertext로 암호화
Dciphering/decryption: ciphertext에서 plaintext로 복구하는 과정
Cryptography: 암호화에 사용되는 연구 분야
Cryptographic system/cipher: 암호 체계
Cryptanalysis: 암호화 세부 사항에 대한 지식없이 메시지를 해독하는데 사용되는 기술
Cryptology: 암호학 및 암호 분석 분야
## 대칭 암호화 모델
![[Pasted image 20250320112608.png|300]]
암호화, 복호화시에 같은 key를 사용함
- 강한 암호화 알고리즘
- Sender와 Receiver가 secret key의 copy를 얻어야 하며, 안전하게 보관해야 한다.

![[Pasted image 20250320112750.png|300]]
### 암호 분석 
공격은 알고리즘의 특성과 일반 텍스트의 일반적인 특성에 대한 약간의 지식에 의존합니다 
공격은 알고리즘의 특성을 활용하여 특정 일반 텍스트를 추론하거나 사용 중인 키를 추론하려고 시도합니다.
### Brute-Force
모든 가능한 ciphertext를 시도하는 것
평균적으로 모든 가능한 key의 절반을 시도하면 성공한다.
## 암호화 체계 보안
- Unconditionally secure
	- 상대가 시간이 많더라도 암호문 해독이 불가능함
- Computationally secure
	- 암호 해독 비용이 암호화된 정보의 가치를 초과
	- 암호 해독 시간이 정보 유효 수명을 초과
# Substitution Technique (대체 기법)
## Monoalphabetic Cipher
### Caesar Cipher
대체 기법의 사용중 가장 먼저 사용되며 간단한 방법
글자를 1 대 1로 대체하는 것 
ex) m을 3글자 이동하여 p e를 3글자 이동하여 h
![[Pasted image 20250320113609.png|300]]
general Caesar algorithm: `C = E(k , p) = (p + k) mod 26`
### Permutation Cipher
- Caesar Cipher와 다르게 무작위로 순서를 변경한다.
- 이러한 방식은 monoalphabetic substitution이라고 불리며, 각 메시지에 대해 하나의 암호 알파벳이 사용된다.
![[Pasted image 20250320114345.png|300]]
#### 단점
알파벳의 빈도 수가 다르고 Digram, Trigram을 이용하여 유추할 수 있다.
- Digram - ex) th
- Trigram - ex) the
![[Pasted image 20250325134020.png|300]]
## Monoalphabetic Cipher :Countermeasure (대응책)
## Multiple-letter ciphers
#### Playfair Cipher
가장 잘 알려진 다중 문자 암호화 기법
plaintext를 두 글자 쌍으로 만들고 이를 하나의 단위로 처리하여 변환한다.
in -> GA, it -> KS 처럼 **같은 알파벳이지만, 다른 문자로 변경된다.**
1. 같은 글자가 연달으면 특정 알파벳(x) 삽입
2. 두 글자를 찾고 직사각형을 그려 같은 행에 있는 꼭짓점으로 바꾼다.
3. 직사각형을 못 그리면 같은 열에서 다음 글자로 바꾼다
4. 직사각형을 못 그리면 같은 행에서 다음 글자로 바꾼다
##### example
MONARCHY라는 keyword를 사용한다고 가정
![[Pasted image 20250325134809.png|200]]
balloon -> balxloon -> IBSUPMNA
ba | lx | lo | on으로 나누고, 1을 적용 후 3,2,3,4의 규칙을 적용해 변경한다.
##### 알파벳 별 상대 빈도
![[Pasted image 20250325135043.png|300]]
## Polyalphabetic Ciphers (다중 알파벳 치환 암호)
단순한 monoalphabetic Cipher보다 발전된 방식으로, plaintext를 암호화하는 동안 여러 개의 다른 단일 알파벳 치환 규칙을 사용
- 서로 관련된 여러 개의 monoalphabetic substitution rule이 사용됨
- key를 통해 특정 변환에 사용할 치환 규칙이 결정됨
### vigenere Cipher
가장 잘 알려지고 간단한 다중 알파벳 치환 암호
#### example
key : deceptive
plaintext: we are discovered save yourself
![[Pasted image 20250325135609.png|300]]
같은 e라도 다르게 번역될 수 있음 (Polyalphabetic Cipher의 특징)
#### 단점
![[Pasted image 20250325135740.png|300]]
key와 plaintext가 같다면, 유추하기가 쉬워질 수 있다.
### vigenere Autokey System
vigenere Cipher의 단점을 해결하기 위해 아래처럼 plaintext를 다시 key로 사용할 수 있다.
![[Pasted image 20250325135901.png|300]]
#### 단점
key와 plaintext는 문자의 동일한 빈도 분포를 공유하기 때문에 통계 기술을 적용할 수 있다.
### vernam Cipher
vernam cipher는 plaintext와 xor을 이용해 encription하고, xor을 다시 돌려 decription을 한다.
![[Pasted image 20250325140127.png|300]]
c = (p+k)mod 2
p = (c-k)mod 2
0 = 0 xor 0
1 = 0 xor 1
1 = 1 xor 0
0 = 1 xor 1
### One-Time Pad
- vernam은 key stream generator를 계속 사용하지만, One-Time Pad는 vernam과 다르게 key를 한 번 쓰고 버린다.
- 메시지와 동일한 길이의 random key를 사용하여 key를 반복해서 사용할 필요가 없다.
- 하나의 메시지를 암호화 및 복호화한 후, 키를 폐기한다.
#### 장점
따라서 unbreakable 방식으로 안전하다. (해독 불가)
평문과 어떤한 통계적 관계도 가지지 않는다.
암호문 자체에 평문에 대한 정보가 전혀 포함되지 않아 해독할 방법이 없다.
#### 단점
- 큰 key의 난수를 계속 만들어야 함
- 보내는 모든 메시지에 대해 동일한 길이의 키가 송신자와 수신자 모두에게 필요함
완전한 안전성을 제공하지만, 실용성이 제한된다.
높은 보안이 필요하며, 낮은 데이터 전송량을 가진 통신에 사용됨
이는 완벽한 보안성을 보장하는 유일한 암호 시스템이다.
## Transposition Cipher
### Rail Fence Cipher
가장 단순한 전치 암호 방식
#### example
if, depth = 2
plain text: meet me after the toga party
![[Pasted image 20250325141422.png|300]]
### Row Transposition Cipher
#### example
key: 4312567
plain text: attack postponed until two am
![[Pasted image 20250325141516.png|300]]
round가 적용될 수 있다
round: 똑같은 변형 규칙을 여러 번 적용하는 것
![[Pasted image 20250325141623.png|300]]
