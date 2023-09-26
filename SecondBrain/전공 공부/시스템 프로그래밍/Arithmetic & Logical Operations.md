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

![[Pasted image 20230927033411.png]]
### 1
addq를 사용하면 Dest가 덮어씌워지므로 leaq를 사용하여 덧셈을 한다.
현재 %rax에는 t1이 들어있다.
### 2
이 뒤에 z와 t1 모두 안 쓰이므로 덮어씌워져도 상관이 없음 따라서 addq를 사용하여도 무방함. 현재 %rax에는 t2가 들어있다.
### 3
y\*48 = 16\*(y + 2y)이므로 3y를 만들어주기 위해 leaq를 사용하는 모습이다.
### 4
4만큼 rdx를 left로 shift하므로 16이 곱해진다
### 5
%rdi + %rdx+4 를 %rcx에 넣는다.
%rdx+4를 연산하는 과정에서 t3도 구할 수 있다.
최종 결과물인 t5도 구할 수 있다.
### 6
imulq를 이용해 실제로 둘을 곱해서 리턴해준다.
%rax에는 현재 rval이 들어가 있다.







