## 메시지 인증 코드 (MAC: Message Authentication Code)
•	다음과 같은 위협을 방지하기 위한 요구사항:
	•	가장 공격(Masquerade)
	•	내용/순서/타이밍 변경
•	기능: 메시지 인증 코드(MAC)
	•	메시지와 비밀 키를 입력으로 받아 고정 길이의 값을 출력하는 함수
	•	이 값이 인증자의 역할을 함
## Basic Uses of Message Authentication code (M A C)
![[Pasted image 20250501203514.png|400]]
## 해시 함수 기반 MAC: HMAC
•	Motivations:
	•	MD5, SHA 같은 암호화 해시 함수는 DES와 같은 대칭 블록 암호보다 소프트웨어에서 일반적으로 더 빠르게 실행됨
	•	암호화 해시 함수용 라이브러리 코드가 널리 사용 가능함
•	HMAC은 IP 보안을 위한 필수 구현 MAC으로 채택됨
•	NIST 표준(FIPS 198-1)으로도 제정됨
## MAC Based on Hash Functions: HMAC
B: block size
L: Hash function output size
ipad: 0x36 repeated
opad: 0x5C repeated
![[Pasted image 20250501203648.png|500]]
## NIST의 FIPS 198-1 -> SP 800-224
- NIST SP 800-224가 FIPS 198-1의 HMAC 사양을 그대로 포함하고 있다
- SP 800-224가 또 다른 문서인 SP 800-107r1의 일부 요구사항도 통합하고 있다
- SP 800-224의 최종 버전이 발표되는 시점에 FIPS 198-1이 공식적으로 철회될 예정
## Data Authentication Algorithm
![[Pasted image 20250501204139.png|300]]
## 암호 기반 MAC: CMAC
•	DAA에 대한 공격: (DAA = CBC MAC + DES)
	•	MAC(K, X) = T이면, MAC(X || (X ⊕ T)) = T도 성립함
	![[Pasted image 20250501204253.png|100]]
•	개선: CMAC
	•	마지막 블록에 대해 파생 키(K1 또는 K2)를 한 번 더 XOR 연산
•	또 다른 개선:
	•	입력에 메시지 길이를 인코딩하여 포함
		이렇게 하면 block을 추가할 수 없음
## Cipher-based MessageAuthentication Code (C M A C)
![[Pasted image 20250501204346.png|300]]
## 인증된 암호화 (AE: Authenticated Encryption)
•	통신의 기밀성과 무결성을 동시에 보호하는 암호 시스템을 지칭하는 용어
•	접근 방식:
	•	인증 후 암호화: CCM
	•	암호화 후 인증: GCM (Galois CTR 모드)
	•	독립적으로 암호화 및 인증: 안전하지 않음
•	각 방식은 복호화와 검증이 모두 명확함
## 카운터 + CBC-MAC 인증된 암호화(CCM)
•	NIST는 IEEE 802.11 WiFi 무선 근거리 통신망의 보안 요구사항을 지원하기 위해 CCM을 표준화함.
•	인증된 암호화를 위한 ~~Encrypt-and-MAC~~ 방식의 변형
	•	Encrypt-and-MAC 대신 사용된 방식: 인증(MAC) 후 암호화 (Authenticate then Encrypt)
	•	NIST SP 800-38C, IETF RFC 3610에 정의됨
•	주요 알고리즘 구성 요소:
	•	AES 암호화 알고리즘
	•	CTR(카운터) 모드의 운영 방식
	•	~~CMAC~~ 인증 알고리즘 
		→ 실제로는 CBC-MAC (암호화된 평문 포함, 메시지 길이 값 포함) 사용
•	단일 키 K가 암호화 및 MAC 알고리즘 모두에 사용됨
## Counter with CBC-MAC (CCM)
![[Pasted image 20250501204625.png|400]]
## SP 800-90Ar1
![[Pasted image 20250501204645.png|500]]