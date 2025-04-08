## AES를 위한 유한체 산술
- AES에서는 모든 연산이 8비트 바이트 단위로 수행된다
- 모든 8비트 숫자는 GF(2⁸)에서의 다항식으로 해석될 수 있다
- 덧셈, 곱셈, 나눗셈 등의 산술 연산은 유한체 GF(2⁸)에서 수행된다
- 사용되는 불가약 다항식: (x⁸ + x⁴ + x³ + x + 1)
## AES의 설계 원칙
SPN 구조 (Subsititution-permutation network)
- 치환(Substitution): SubBytes, MixColumns, AddRoundKey
- 순열(Permutation): ShiftRows

