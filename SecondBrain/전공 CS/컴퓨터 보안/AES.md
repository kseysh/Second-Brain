## AES를 위한 유한체 산술
- AES에서는 모든 연산이 8비트 바이트 단위로 수행된다
- 모든 8비트 숫자는 GF(2⁸)에서의 다항식으로 해석될 수 있다
- 덧셈, 곱셈, 나눗셈 등의 산술 연산은 유한체 GF(2⁸)에서 수행된다
- 사용되는 불가약 다항식: (x⁸ + x⁴ + x³ + x + 1)
## AES의 설계 원칙
SPN 구조 (Subsititution-permutation network)
- 치환(Substitution): SubBytes, MixColumns, AddRoundKey
- 순열(Permutation): ShiftRows
## AES-128 Encryption / Decryption
![[Pasted image 20250408222338.png|300]]
- 0 라운드: Add Round Key w\[0] ~ w\[3] 사용
- 1-9 라운드
	- Substitute Bytes
	- Shift Rows
	- Mix Columns
	- Add Round key
- 10 라운드 
	- Substitute Bytes
	- Shift Rows
	- Add Round key
Decryption은 이에 역연산으로 동작함
## AES-128: 상세 구조
•	매 라운드에서 전체 데이터 블록을 하나의 매트릭스로 처리하며, 치환과 순열 연산을 수행함
•	입력으로 제공된 키는 32비트 워드 44개로 확장됨 (`w[i]`)
•	4가지 주요 단계
1.	Substitute Bytes: S-Box를 사용하여 바이트 단위 치환 수행
2.	ShiftRows: 간단한 바이트 위치 재배열 (순열)
3.	MixColumns: GF(2⁸) 산술을 사용하는 열 기반 치환
4.	AddRoundKey: 현재 블록과 확장된 키의 일부를 비트 단위 XOR 수행
## Substitute byte transformation
![[Pasted image 20250408222937.png|300]]
## Add round key transformation
![[Pasted image 20250408222951.png|300]]

## S-Box
- S-Box
![[Pasted image 20250408230810.png|300]]
- Inverse S-Box
![[Pasted image 20250408230823.png|300]]
### S-Box 설계 이유
•	S-Box는 알려진 암호 해독 공격에 강하도록 설계됨
•	입력 비트와 출력 비트 사이의 상관 관계를 낮추고, 출력이 입력의 선형 함수가 아니도록 설계됨
•	비선형성(nonlinearity)은 곱셈 역원 연산을 통해 제공됨

### example
![[Pasted image 20250408231338.png|500]]
입력 바이트: 0x83
1. 0x83의 역원을 GF(2⁸)에서 구함 = x⁷, 즉 10000000
	1. 2진수로 10000011, 즉 다항식 형태로 x⁷ + x + 1
	2. modulo는 AES에서 정의한 불가약 다항식 x⁸ + x⁴ + x³ + x + 1을 사용
2. W = A • V ⊕ B
3. 최종 결과인 W는 2진수 11101100, 즉 0xEC

역연산
1. S-Box의 출력값을 다시 입력값으로 사용
2. Byte -> bit vector로 전환
3. V = A<sup>-1</sup>(W ⊕ B)
4. GF(2⁸)에서 역원 계산
## S-Box 설계 이유
S-Box는 알려진 암호 해독 공격에 강하도록 설계됨
입력 비트와 출력 비트 사이의 상관 관계를 낮추고, 출력이 입력의 선형 함수가 아니도록 설계됨
비선형성(nonlinearity)은 곱셈 역원 연산을 통해 제공됨
## AddRoundKey 변환
•	상태(State)의 128비트를 Round key의 128비트와 비트 단위 XOR 수행
•	이 연산은 각 열의 4바이트와 라운드 키의 하나의 워드 간의 연산으로 볼 수 있음
•	바이트 수준의 연산으로도 볼 수 있음

설계 이유:
•	매우 단순하지만, 상태(State)의 모든 비트에 영향을 미침
•	키 확장의 복잡성과 다른 AES 단계들과 결합되어 보안을 확보함

## AES Row and Column Operations
![[Pasted image 20250409134309.png|500]]
### Shift Row transformation
행 간 이동 횟수가 정해져 있음, 역 연산시에는 반대로 하면 됨
- 한 열의 4 바이트가 서로 다른 열로 분산되도록 하여 혼합 효과를 제공
### Mix column transformation
특정 행렬 A와 행렬을 곱함, 역 연산시에는 A의 역행렬을 곱하면 다시 돌아감
- 코드 단어 간의 최대 거리(Maximum Distance)를 가진 선형 코드 기반 행렬 계수를 사용하여 각 열 내 바이트들을 잘 섞이도록 보장
- ShiftRows와 결합되어, 몇 라운드만 거치면 모든 출력 비트가 모든 입력 비트에 의존하도록 함
## AES Encryption Round
![[Pasted image 20250409134913.png|500]]
## Inputs for Single AES Round
![[Pasted image 20250409135127.png|400]]
S-box와 MixColmns matrix는 상수 값으로, 공격자도 알고 있음
## AES-128 Key Expansion
•	4워드(16바이트) 입력 키를 받아 44워드(176바이트)의 선형 배열로 확장
•	초기 AddRoundKey 및 10개의 라운드 각각에 4워드 키 제공
•	키의 처음 4워드는 그대로 확장 키에 복사됨
•	이후의 확장 키는 매번 4워드 단위로 계산됨
•	각 워드 `w[i]`는 바로 앞의 `w[i-1]`과 `w[i-4]`에 기반
•	4개 중 3개는 단순 XOR
•	w 배열에서 인덱스가 4의 배수인 경우 더 복잡한 함수 사용
![[Pasted image 20250409135310.png|400]]
### 설계 이유
•	Rijndael 개발자들은 키 확장 알고리즘도 암호 해독 공격에 강하도록 설계함
•	라운드에 따라 달라지는 상수를 포함시켜, 라운드 키 생성 방식 간의 대칭성을 제거
•	키 일부 또는 라운드 키 일부만 알더라도 다른 라운드 키 비트를 계산할 수 없도록 함 (비선형성)
