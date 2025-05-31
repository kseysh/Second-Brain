## 메시지 인증 코드 (MAC: Message Authentication Code)
•	다음과 같은 위협을 방지하기 위한 요구사항:
	•	가장 공격(Masquerade)
	•	내용/순서/타이밍 변경
•	기능: 메시지 인증 코드(MAC)
	•	메시지와 비밀 키를 입력으로 받아 고정 길이의 값을 출력하는 함수
	•	이 값이 인증자의 역할을 함
## Basic Uses of Message Authentication code (MAC)
![[Pasted image 20250501203514.png|400]]
2번째 방법: 
K1으로 메시지 M에 대해 MAC을 생성 K2로 M과 MAC을 Encrypt -> 인증, 기밀성 충족
3번째 방법:
K2로 M Encrpt, K1으로 암호문 E(K2, M)에 대해 MAC 생성
## Hash function based MAC: HMAC
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
Steps 1~3: K₀ 만들기
	1.	K의 길이가 B와 같으면 → K₀ = K
	2.	K가 B보다 길면 → K₀ = H(K)로 압축하고 0으로 패딩하여 B바이트로 맞춤
	3.	K가 B보다 짧으면 → 0을 덧붙여 B바이트로 만듦

Step 4: 내부 키 생성
	•	K₀ ⊕ ipad
K₀와 ipad를 XOR하여 내부 키를 생성

Step 5: 내부 키에 메시지 추가
	•	(K₀ ⊕ ipad) || text
내부 키 뒤에 메시지를 붙임

Step 6: 내부 해시 계산
	•	H((K₀ ⊕ ipad) || text)
결과를 해시 함

Step 7: 외부 키 생성
	•	K₀ ⊕ opad
K₀와 opad를 XOR하여 외부 키 생성

Step 8: 외부 키에 내부 해시 추가
	•	(K₀ ⊕ opad) || H(...)
외부 키 뒤에 내부 해시를 붙임

Step 9: 최종 HMAC 계산
	•	H((K₀ ⊕ opad) || H((K₀ ⊕ ipad) || text))
## NIST의 FIPS 198-1 -> SP 800-224
- NIST SP 800-224가 FIPS 198-1의 HMAC 사양을 그대로 포함하고 있다
- SP 800-224가 또 다른 문서인 SP 800-107r1의 일부 요구사항도 통합하고 있다
- SP 800-224의 최종 버전이 발표되는 시점에 FIPS 198-1이 공식적으로 철회될 예정
## Data Authentication Algorithm
![[Pasted image 20250501204139.png|300]]
## Cipher-based MAC: CMAC
•	DAA에 대한 공격: (DAA = CBC MAC + DES)
	•	MAC(K, X) = T이면, MAC(X || (X ⊕ T)) = T도 성립함
	![[Pasted image 20250501204253.png|100]]
•	개선: CMAC
	•	마지막 블록에 대해 파생 키(K1 또는 K2)를 한 번 더 XOR 연산
•	또 다른 개선:
	•	입력에 메시지 길이를 인코딩하여 포함
		이렇게 하면 block을 추가할 수 없음
## Cipher-based MessageAuthentication Code (CMAC)
![[Pasted image 20250501204346.png|300]]
## 인증된 암호화 (AE: Authenticated Encryption)
•	통신의 기밀성과 무결성을 동시에 보호하는 암호 시스템을 지칭하는 용어
•	접근 방식:
	•	인증 후 암호화: CCM
	•	암호화 후 인증: GCM (Galois CTR 모드)
	•	독립적으로 암호화 및 인증: 안전하지 않음
•	각 방식은 복호화와 검증이 모두 명확함
## Counter + CBC-MAC (CCM)
=> Encrypt는 counter mode로 하고, 암호화는 CBC-MAC을 사용하겠다는 의미
•	NIST는 IEEE 802.11 WiFi 무선 근거리 통신망의 보안 요구사항을 지원하기 위해 CCM을 표준화함.
•	인증된 암호화를 위한 ~~Encrypt-and-MAC~~ 방식의 변형
	•	Encrypt-and-MAC 대신 사용된 방식: 인증(MAC) 후 암호화 (Authenticate then Encrypt)
	•	NIST SP 800-38C, IETF RFC 3610에 정의됨
•	주요 알고리즘 구성 요소:
	•	AES 암호화 알고리즘
	•	CTR mode of operation
	•	~~CMAC~~ 인증 알고리즘 
		→ 실제로는 CBC-MAC (암호화된 평문 포함, 메시지 길이 값 포함) 사용
•	단일 키 K가 암호화 및 MAC 알고리즘 모두에 사용됨
## Counter with CBC-MAC (CCM)
![[Pasted image 20250501204625.png|400]]
위 부분: 인증하기 위한 tag를 생성하는 부분
아래 부분: Encryption하는 부분
MAC 생성과정에 의해 생성된 TAG를 Plain text로 보고, 원래의 plain text와 같이 모아서 encryption하는 것으로 볼 수 있음
## SP 800-90Ar1
![[Pasted image 20250501204645.png|500]]
### Hash_DRBG
V는 엔트로피 입력을 기반으로 생성된 시드 값
C = Hash(0x00 || V)
- 순수 해시 함수를 반복적으로 사용
Counter 값을 점점 증가시키면서 Hash Function에 V + counter를 넣고 해시 반복
결과를 이어붙여 충분한 길이의 pseudo random bit를 생성
### HMAC_DRBG
- 초기 Key: 0x00..00
- 초기 V: 0x01..01
Pseudo Random
- HMAC을 반복 호출하며 V 값을 입력으로 사용
- 출력된 블록들을 이어붙여 충분한 길이의 난수 비트를 생성
- 각 반복마다 V는 업데이트되며, 이는 추후 상태에도 영향을 줌
=> HMAC을 반복하여 원하는 길이만큼 의사난수 생성
## Summary
MAC이 무엇인지
Hash를 기반으로 MAC을 어떻게 만드는 지
CBC-MAC이 문제가 있으니 어떻게 개선하는지
인증과 암호화를 동시에 하는 두 가지 방법
Hash 또는 HMAC으로 난수 생성을 어떻게 하는지
