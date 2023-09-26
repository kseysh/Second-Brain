## Fractional Binary Numbers
ex)
1011.101 => 8+2+1+1/2+1/8

### 한계
정확한 실수표기가 어렵다.
소수점을 어디에 찍어야할지 결정이 어렵다. (스케일 vs 소수점의 정확도)

# Floating Point
점이 떠다닌다는 의미
> IEEE : 전자적 표준 기관

![[Pasted image 20230926144416.png]]
s : 음수 or 양수
E : Scale
M : 유효 숫자 (정확도)
### 장점
overflow, underflow, rounding을 컨트롤하기 매우 편리하다
### 단점
하드웨어로 구현하기 복잡하다.

![[Pasted image 20230926150742.png]]

## float Floating-Point Values
![[Pasted image 20230926151046.png]]
exp가 0이거나 255일 때는 특별한 의미를 가진다.

### Normalized Values
#### exp
exp : 1~254
E = exp - 127 (E는 음수일 수도 있기 때문)
#### frac
어차피 1.xxxx 이므로 1은 무시하고 작성한다.

ex)
![[Pasted image 20230926152053.png]]

### Denormalized Values
0.xxxxx \* 2<sup>-126</sup> 이렇게 계산할 수 있다
#### normalized values와 구분한 이유
0이 들어갈 수 없기 때문 ( normalized는 자동으로 frac 1이 들어가기 때문에 ) 
#### denormalized values의 특징
+0, -0 둘 다 있다.

##### exp = 111.....1, frac = 000...0
0을 나타낸다
##### exp = 111.....1, frac ≠ 000...0
0.0에 가까운 값
### Special Values
#### exp = 111.....1
##### exp = 111.....1, frac = 000...0
무한을 나타낸다.
overflow가 발생할 때의 표현이다.
positive, negative 모두 가능하다.

##### exp = 111.....1, frac ≠ 000...0
Not-a-Number (NaN)
ex) sqrt(-1) 무한, -무한, 무한 \* 0

ex)
![[Pasted image 20230926154554.png]]
exp가 4-bits이므로 bias가 7이다.

![[Pasted image 20230926154742.png]]
0.111 \* 2<sup>-6</sup>
에서
1.000 \* 2<sup>-6</sup>
로 바뀌는 구간이 존재한다.
> 224 -> 240으로 넘어가는 구간이 있는 것 처럼 소멸되는 구간도 있다.

### Distribution of values
큰 수에서는 조금 듬성듬성 값이 있고, 작은 수에서는 촘촘하게 값이 존재한다.

![[Pasted image 20230926155046.png]]

## IEEE Encoding (Floating Point)의 장점

0의 bit level value가 int와 똑같다.
값의 대소 비교가 쉽다.

## Integer vs Floating Point
![[Pasted image 20230926161754.png]]
### case 1
exp = E  + bias = 21 + 127 = 148이다.
### case 2
exp = E  + bias = 30 + 127 = 157이다.
frac에서 길어서 넣을 수 없는 부분은 날라가게 된다.

# Floating Point Operations

## Floating Point Operations의 기본 아이디어
정확하게 표현하기는 어려우므로 계산하고 올림을 적용한다.
![[Pasted image 20230926162252.png]]

앞의 유효숫자끼리 곱하고, 뒤의 유효숫자끼리 곱한다.
ex)
![[Pasted image 20230926162409.png]]
곱셈 시에는 frac부분이나 exp 부분이 길어진다. 

- frac부분이 길어질 때 :
짤리는 부분도 생기게 되며, 그럴 때에는 반올림을 해준다.
- exp 부분이 길어질 때 :
overflow -> infinity or 0으로 처리해준다.

## Round-To-Even 
5를 올림했을 때 짝수이면 올린다. (홀수이면 버린다.)
컴퓨터에서는 default로 이 방법을 사용하여 올림을 적용한다.
ex)
0.5 -> 0
1.5 -> 2
2.5 -> 2
3.5 -> 4
### 이진수에서의 짝수
최하위 bit가 0이면 짝수이다.
### 이진수에서 0.5
유효숫자 밑이 정확히 100....일 때 0.5이다.

ex)
빨간색이 유효숫자 밖으로 나와야하는 숫자일 때,
![[Pasted image 20230926163817.png]]
#### case 1
버려야하는 숫자가 100보다 작음 => 내림
#### case 2
버려야하는 숫자가 100보다 큼 => 올림
#### case 3
버려야하는 숫자가 100이고, 앞의 숫자가 1로 끝남 => 올림
#### case 4
버려야하는 숫자가 100이고, 앞의 숫자가 0으로 끝남 => 내림

## Floating Point의 곱셈

![[Pasted image 20230926171520.png]]

s : XOR 연산
M과 E는 곱셈시에 길어질 수 있다.
E가 범위를 벗어나면 overflow가 발생한다.
frac의 정확도를 맞추기 위해 M을 [[#Round-To-Even]]을 사용하여 반올림 해준다.

### Floating Point 곱셈의 수학적 특성
- 닫힌 연산이다.
- 교환법칙이 성립한다
- 결합법칙이 성립하지 않는다. (overflow의 가능성이 있기 때문)
![[Pasted image 20230926172138.png]]
- 덧셈의 분배법칙이 적용되지 않는다. (overflow의 가능성이 있기 때문)
![[Pasted image 20230926172425.png]]
- 1은 곱했을 때 자기 자신이 나온다. (곱셈의 항등원이다.)
- 단조성(monotonic)을 만족한다. (int는 overflow가 발생할 수 있어 만족하지 않는다.)
	- 대부분 만족하지만, 무한대와 NaN은 단조성을 만족하지 않는다.
	- ![[Pasted image 20230926172629.png]]

## Floating Point의 덧셈
![[Pasted image 20230926173247.png]]

E를 큰 쪽에 맞춰서 더해준다. 
ex)
>1.01 \* 2<sup>5</sup> 
>1.01 \* 2<sup>-1</sup> 
>이 둘을 더하면 아래 값을 0.00000101 * 2<sup>5</sup>으로 바꿔서 계산해준다.

1≤M<2 여야 하므
만약 M ≥ 2면, M을 오른쪽으로 shift하고, E를 증가시킨다. 
만약 M < 1이면, M을 k만큼 왼쪽으로 shift하고, k만큼 E를 감소시킨다.
E가 overflow가 나면 inf나 0으로 맞춰준다.
frac 정확도를 맞추기 위해 M을 반올림 해준다.

### Floating Point 덧셈의 수학적 특성
- 닫힌 연산이다. (Floating Point는 항상 닫힌 연산이다. 결과가 Floating Point로 표현되므로)
> 닫힌 연산 : 모든 값이 Floating Point로 표현될 수 있다.
- 교환 법칙이 성립한다
- 결합 법칙은 성립하지 않는다. (overflow가 발생할 수 있기 때문)
ex) ![[Pasted image 20230926180717.png]]
- 0은 했을 때 자기 자신이 나온다. ( 0이 덧셈의 항등원이다. )
- 대부분의 요소가 역원이 존재한다.
	- 역원 : 더했을 때 0이 나오는 수
	- 예외 : inf, NaNs
- 단조성(monotonic)을 만족한다.
	- 대부분 만족하지만, 무한대와 NaN은 단조성을 만족하지 않는다.
	- ![[Pasted image 20230926181049.png]]

## C에서의 Floating Point
### Conversions / Casting
#### double/float -> int
int의 표현 범위를 넘어가거나 NaN일 때는 정의되지 않는다. 보통 TMin으로 캐스팅된다.
소수점을 버린다.
Round-To-Even으로 반올림한다.
#### int -> double
double의 frac이 52-bits이므로 int를 모두 표현할 수 있다.
#### int -> float
float의 frac이 23-bits이므로 int를 모두 표현하지 못한다.
Round-To-Even을 통해 숫자를 버리게 되는 상황이 올 수도 있다.

## Floating Point Example
![[Pasted image 20230926185357.png]]
### Case 1
False. x = TMax일 때 float는 frac이 23bit 밖에 지원하지 않으므로 유효숫자가 날아갈 수 있음
### Case 2
True. double의 정밀도가 int보다 크므로 
### Case 3
True. double의 정밀도가 float보다 크므로
### Case 4
False. float의 정밀도가 double보다 작으므로
### Case 5
True. float는 sign bit만 바꾸면 부호가 바뀐다.
### Case 6
False.
int/int = int(몫을 반환)
int/FP = FP(나눗셈한 값을 반환)
### Case 7
True. overflow가 발생해도 -inf로 되므로 0보다 작다. (단조성이 유지가 되기 때문)
### Case 8
True. -붙으면 그대로 -가 붙으므로 맞다.
### Case 9
True. overflow가 발생하면 inf로 되므로 0보다 크다. (단조성이 유지가 되기 때문)
### Case 10
False. d가 매우 크고, f가 매우 작은 경우 f의 유효숫자가 날라갈 수 있다.






