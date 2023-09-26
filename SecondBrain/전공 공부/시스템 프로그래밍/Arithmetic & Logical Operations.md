# `leaq` Source, Dest
leaq = Load Effective Address 
movq (  ), %~ 는 주소에 가서 주소에 있는 값을 register에 넣는다면, 
leaq (  ), %~ 는 주소만 계산해서 주소를 register에 넣는다. (address만 load하는 것)
- Source는 메모리, Dest는 register가 오게 된다.

### 사용 하는 곳
ex ) p = &x\[i];
주소 값을 어딘가에 넣을 때 사용한다.
x + k\*y 를 계산할 때 사용한다. (곱하기보다 빠른 연산이기 때문이다.)
k = 1,2,4,8등...
![[Pasted image 20230927031907.png]]
x \* 12 = 4 \* ( x + 2x )
leaq (%rdi, %rdi, 2)를 사용하면 3x를 구할 수 있다.
salq (shift arithmetic left) $2, %rax => 3x를 왼쪽으로 두 칸 이동 (4를 곱하는 연산)

### Arithmetic Operations
![[Pasted image 20230927032405.png]]
Src와 Dest를 계산해 Dest에 저장한다.
부호가 있느냐 없느냐에 따라 Arithmetic shift와 Logical shift로 나뉜다.
여기에서는 signed와 unsigned의 구분이 없다. (연산 자체에서는 signed와 unsigned가 구분이 없으므로) 
![[Pasted image 20230927032832.png]]

## Arithmetic Expression Example



