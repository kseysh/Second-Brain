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
그림처럼 0001에서 1111을 빼려고 하면, 음수/양수를 표현하기 위해 가상의 한개의 bit(Carry)가 하나 생기게 된다. 따라서 Carry가 생겨 CF가 있다고 볼 수 있다.

![[Pasted image 20230928032453.png]]
위 그림에서도 계산을 위해서 가상의 한개의 bit를 만들었지만 Carry라는 것은 원래 bit보다 상위의 한 bit만을 뜻하고, 현재 그 bit는 0이므로 CF가 없다고 볼 수 있다.
(-) - (+) = (-)이므로 OF도 없다.
Unsigned로 계산했을 때는 overflow가 일어난 상태, signed로 해석했을 때는 overflow가 안 일어난 상태이다. 연산 자체는 같지만 해석을 하는데에 있어서 signed와 unsigned에서 어떤 Flag를 지표로 삼느냐에 따라 Unsigned는 CF를 확인하고, Signed는 SF와 OF를 확인한다.
### ZF
값이 0이므로 두 값이 같다라고 판단할 수 있다.
### SF
Sign Flag가 설정되었다는 것은 Src1-Src2가 음수라는 것이며, 따라서 Src2가 더 큰 값을 가진다고 판단할 수 있다.
### OF
양수 + 양수가 결과가 양수이므로 OF는 설정되지 않는다.

## Condition Codes (Explicit Setting: Test)
## `testq Src2, Src1`
testq Src2, Src1는 dest 설정하지 않는 컴퓨터의 a&b와 같다.

더하거나 하는 연산이 아니므로 CF와 OF가 생길 수 없다.

### ZF
a&b == 0 일 때 세팅된다.
### SF
a&b < 0 일 때 세팅된다.

## SetX instructions
`setX Dst`
Condition을 보고 확인할 수 있다.
![[Pasted image 20230928035215.png]]
e = equal
n = not
s = sign
g = greater than
l = less than
ex ) 
sete는 Flag가 equal인 상태이면 Dst를 1로 만들겠다는 의미
setg는 Flag가 greater than이면 Dst를 1로 만들겠다는 의미 
cmpq Src2, Src1
setg Dst => 위의 있는 연산이 더 크다라는 의미이면 Dst를 1로 만들겠다.
이면, Src1이 Src2보다 크면 Dst를 1로 만들겠다는 의미이다.
즉 cmpq Src2, Src1이 ~(SF^OF) | ZF 이면 Dst를 1로 만들겠다는 의미이다.

### setl 
`cmpq b,a`
`setl` 일 때,
signed에서 대소 비교를 위해 사용한다.
setl일 때 알고 싶은 것 : a<\b인 상황
CF : signed에서는 Carry가 생기는 것이 overflow에 연관있는 것이 아니므로 신경쓰지 않는다.
ZF : ZF가 설정되어 있으면 같다라는 의미
SF : a<\b이려면 SF는 설정되어 있어야 한다.(1이어야 한다.) but OF가 설정되지 않았을 때, SF가 설정되어야 a<\b이다.

### setle
signed에서 대소 비교를 위해 사용한다.












