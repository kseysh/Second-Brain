# Condition Codes
- CF Carry Flag (for unsigned) 
	- Carry가 생겼는가 안 생겼는가
	- Carry : 연산을 해서 한 bit가 더 생겼는지
- ZF Zero Flag
	- 결과가 0인가 아닌가
	- t == 0 인지를 검사함으로써 확인
- SF Sign Flag (for signed)
	- 음수인가 아닌가
	- 최상위 bit가 1인지 아닌지를 확인함으로써 확인
- OF Overflow Flag (for signed)
	- two's complement에서 오버플로우가 있었는지 없는지
	- (+) \+ (+) = (-) , (-) \+ (-) = (+) 일 때 오버플로우라고 한다.
ex)
![[Pasted image 20230925164144.png]]
- 1 번째 예시
CF는 Carry가 있으므로 있음
0이 아니므로 ZF는 없음
최상위 bit가 1이 아니므로 SF도 없음
원본 데이터가 음수 음수인데 값도 음수가 나왔으므로 OF는 설정되지 않음

- 2 번째 예시
CF는 Carry가 있으므로 있음
0이므로 ZF 설정
최상위 bit가 1이 아니므로 SF 없음
원본 데이터가 음수 양수인데 값도 양수가 나왔으므로 OF는 설정되지 않음

- 3 번째 예시
CF는 Carry가 있으므로 있음
0이 아니므로 ZF는 없음
최상위 bit가 1이 아니므로 SF도 없음
원본 데이터의 최상위 bit가 1 1인데 값이 0이 나왔으므로(overflow 발생) OF 설정

> leaq 연산에서는 Flag가 설정되지 않는다.

![[Pasted image 20230927191034.png]]
SF. 결과의 최상위 비트가 1이므로
OF. 양수 + 양수 가 음수가 나왔으므로


## Condition Codes (Explicit Setting: Compare)
## `cmpq Src2, Src1`
Src1이 Src2보다 크냐, 작냐를 비교하는 Condition Codes
Src1 - Src2를 해서,
양수이면 Src1이 크다.
음수이면 Src2가 크다.
0이면 같다의 결과를 낼 수 있다.
뺐을 때 Flag가 어떻게 설정되는지만 궁금한 것

### CF
Unsigned Number의 overflow를 표현한다.

ex)
![[Pasted image 20230927193100.png]]
그림처럼 0001에서 1111을 빼려고 하면, 음수/양수를 표현하기 위해 가상의 한개의 bit(Carry)가 하나 생기게 된다. 따라서 Carry가 생긴다.

![[Pasted image 20230928032453.png]]
위 그림에서는 


### ZF
값이 0이므로 두 값이 같다라고 판단할 수 있다.
### SF
Sign Flag가 설정되었다는 것은 Src1-Src2가 음수라는 것이며, 따라서 Src2가 더 큰 값을 가진다고 판단할 수 있다.
### OF
양수 + 양수가 결과가 양수이므로 OF는 설정되지 않는다.