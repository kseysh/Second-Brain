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
#### normalized values와 구분한 이유
0이 들어갈 수 없기 때문 ( normalized는 자동으로 frac 1이 들어가기 때문에 ) 



















