# Condition Codes
- CF Carry Flag (for unsigned) 
	- Carry가 생겼는가 안 생겼는가
	- Carry : ?
- ZF Zero Flag
	- 결과가 0인가 아닌가
	- t == 0 인지를 검사함으로써 확인
- SF Sign Flag (for signed)
	- 음수인가 아닌가
	- t < 0인지를 검사함으로써 확인
- OF Overflow Flag (for signed)
	- 오버플로우가 있었는지 없는지
	- signed의 오버플로우를 검사함으로써 확인
ex)
![[Pasted image 20230925164144.png]]
- 1 번째 예시
CF는 ?
0이 아니므로 ZF는 없음
최상위 bit가 1이 아니므로 SF도 없음
원본 데이터가 음수 음수인데 값도 음수가 나왔으므로 OF는 설정되지 않음

- 2 번째 예시
CF는 ?
0이므로 ZF 설정
최상위 bit가 1이 아니므로 SF 없음
원본 데이터가 음수 양수인데 값도 양수가 나왔으므로 OF는 설정되지 않음

- 3 번째 예시
CF는 ?
0이 아니므로 ZF는 없음
최상위 bit가 1이 아니므로 SF도 없음
원본 데이터의 최상위 bit가 1 1인데 값이 0이 나왔으므로 OF 설정

> leaq 연산에서는 Flag가 설정되지 않는다.

