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
## 메시지 해독 공격 타입
암호문 단독 공격 (Ciphertext Only)
•	암호화 알고리
## Types of Attacks on Encrypted Messages
known plaintext: 내용을 조금 아는 것
## Substitution Technique

### Caesar Cipher
글자를 1 대 1로 대체하는 것 
ex) m을 3글자 이동하여 p e를 3글자 이동하여 h
![[Pasted image 20250320113609.png|300]]
// 여기까지
### Monoalphabetic Cipher
![[Pasted image 20250320114345.png|300]]