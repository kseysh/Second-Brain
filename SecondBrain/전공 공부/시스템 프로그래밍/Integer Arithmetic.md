# Addition

![[Pasted image 20230925155816.png]]
## Unsigned Addition (UAdd<sub>w</sub>)
w bit의 unsigned data의 덧셈은 UAdd<sub>w</sub> = ( u + v ) mod 2<sup>w</sup> 로 나타내진다. 2<sup>w</sup>를 넘으면 mod 연산이 된다.

![[Pasted image 20230925153851.png]]

## Two's Complement Addition (TAdd<sub>w</sub>)
Unsigned와 덧셈 과정은 동일하지만(bit level에서는 계산이 같다) 그것을 Two's Complement로 해석한다는 것이 다르다.

### TAdd<sub>w</sub>의 Overflow의 기준
> 같은 부호의 두 값을 더해서 다른 부호가 나오면 overflow이다.
- (+) + (+)가 (-)가 나오면 overflow
ex)
![[Pasted image 20230925154730.png]]
- (-) + (-)가 (-)가 나오면 overflow가 아님
ex)
![[Pasted image 20230925154627.png]]
- (-) + (-)가 (+)가 나오면 overflow
- (+) + (-)는 overflow가 나올 수 없다. ( 절댓값이 작아지는 방향으로 더해지기 때문 => 원래 표현할 수 있는 범위 내에 무조건 들어가게 된다. )

# Multiplication

![[Pasted image 20230925155833.png]]
## Unsigned Multiplication (UMult<sub>w</sub>)
UMult<sub>w</sub>(u, v) = (u \* v) mod 2<sup>w</sup>
overflow를 해결하고 싶으면 두 배의 데이터 타입을 갖는 형태로 캐스팅 해준 후 계산하면 된다.

## Signed Multiplication (TMult<sub>w</sub>)
TMult<sub>w</sub>(u, v) = (u \* v) mod 2<sup>w</sup> 
Unsigned와 Signed의 곱셈은 원래 다르게 동작해야 하지만, 작은 bit에서는 같은 결과를 나타낸다.
따라서 Unsigned 처럼 곱하고 상위 bit를 지운다.

## Unsigned와 Signed의 곱셈 과정이 같아도 되는 이유

![[Pasted image 20230925160413.png]]
그림을 보면, bit-level에서는 signed의 곱셈이나, unsigned의 곱셈이나 Truncated를 한다면, 같다.
원래 bit-level에서의 곱셈은 Unsigned의 곱셈처럼 하는 것이 맞지만 Truncated를 하면 어차피 같은 값이 되므로 Unsigned와 Signed의 곱셈 과정이 같은 것이다.

## 상수의 곱셈 / 나눗셈
multiplication으로 계산하면 오래 걸리기 때문에, Shift 연산을 통해 계산을 수행한다.
### Signed/Unsigned에서 2를 Shift를 통해 곱셈
signed와 unsigned 모두 과정이 같다.
![[Pasted image 20230925161240.png]]
u << k = u \* 2<sup>k</sup> 
ex)
![[Pasted image 20230925161513.png]]

### Unsigned에서 2를 Shift를 통해 나눗셈
Unsigned에서만 유효하다.
![[Pasted image 20230925161906.png]]
2로 나누고 버림을 수행한다.

ex)
![[Pasted image 20230925161936.png]]


