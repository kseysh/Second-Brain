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
Cryptanalysis: 암호화 세부 사항에 
Cryptology: 
암호화 – 암호화에 사용되는 많은 체계의 연구 분야 • 암호화 시스템/암호 – 체계 • 암호 분석 – 암호화 세부 사항에 대한 지식 없이 메시지를 해독하는 데 사용되는 기술 • 암호학 – 암호학 및 암호 분석 분야
## Simplified Model of Symmetric Encryption
![[Pasted image 20250320112608.png|300]]
암호화, 복호화시에 같은 key를 사용함


![[Pasted image 20250320112750.png|300]]

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