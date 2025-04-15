## Electronic Codebook Mode (ECB)
![[Pasted image 20250410213232.png|400]]

![[Pasted image 20250410213310.png|200]]
ECB는 동일한 평문 블록이 항상 동일한 암호문 블록으로 암호화되기 때문에, 평문에 반복되는 패턴이 있다면 암호문에서도 그대로 노출됨.
## Cipher Block Chaining Mode (CBC)
![[Pasted image 20250410213638.png|400]]

CBC는 각 블록이 이전 암호문 블록과 XOR 연산 후 암호화되기 때문에, 같은 평문 블록이라도 암호문이 달라짐. 이로 인해 패턴이 감춰지고 보안성이 증가함.
Message Auth Code(MAC)에서 사용
Plain text를 Cipher text로 바꾸기 위해 key가 필요함, key가 있으면 위조가 가능하기 때문

### MAC의 Security Services 특성
Integrity: 오류 발생 시 발견할 수 있는 mechanism을 가지고 있음
Authenticity: key를 가진 사람만 보낼 수 있기 때문에 Authenticity를 가짐
Nonrepudiation X : key를 알고 있으니까 key를 알고있는 사람이 위조를 해버리면 부인 방지를 못함
## s-bit Cipher Feedback Mode (CFB)
![[Pasted image 20250410214311.png|400]]
Decrytion할 때, Encrypt 함수를 그대로 사용해도 된다.
CBC와 비슷하지만 Stream mode이다.
## Output Feedback (OFB)
![[Pasted image 20250410220914.png|400]]
Pre-processing 가능
병렬성을 이용할 수 없음
Cipher text가 feedback으로 전달되지 않아 Plain text가 cipher text가 될 때 한 block에만 영향을 주기 때문에, error 감지가 어려움
## Counter Mode (CTR)
전처리 가능
병렬 처리 가능
Random Access 가능
![[Pasted image 20250410220939.png|400]]

## Feedback 특성
![[Pasted image 20250410221022.png|500]]

![[Pasted image 20250410221035.png|500]]

## 암호 모듈 검증 제도
국정원에 암호 모듈 검증 제도라는 것이 있다.
공공기관에서 암호 쓸려면 인증받은 것만 써야함
ARIA, SEED, LEA, AES 사용 가능

- ARIA
	- AES와 유사
- SEED
	- DES와 유사
- LEA
	- S-box 없음