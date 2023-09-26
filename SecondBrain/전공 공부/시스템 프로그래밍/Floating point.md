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

### Distribution of values
큰 수에서는 조금 듬성듬성 값이 있고, 작은 수에서는 촘촘하게 값이 존재한다.

![[Pasted image 20230926155046.png]]

## IEEE Encoding (Floating Point)의 장점

0의 bit level value가 int와 똑같다.
값의 대소 비교가 쉽다.

## Integer vs Floating Point
![[Pasted image 20230926161754.png]]
### 





