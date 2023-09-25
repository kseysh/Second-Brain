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

## Signed Multiplication (TMult)


