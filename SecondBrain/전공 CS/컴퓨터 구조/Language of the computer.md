## MIPS Instruction Set
### ISA의 두 가지 종류
#### CISC (Complex IS Computer)
사용 분야: Desktop, Server
ex) x86
#### RISC (Reduced IS Computer)
사용 분야: Mobile, Embeded
ex) MIPS, ARM
## Arithmetic Operations
### `add a, b, c`
a = b + c
### `sub a, b, c`
a = b - c
#### example 
`f = (g + h) – (i + j);`
- Compiled MIPS code:
	- add t0, g, h 
	- add t1, i, j 
	- sub f, t0, t1 
## Register Operands
Arithmetic instrucion은 register operand를 사용한다.
- MIPS는 32x32 bit register file을 갖는다.
	- 빈번하게 접근되는 데이터에 사용된다
	- 0-31까지의 번호로 이루어짐
	- 32 bit data 단위로 이루어지는데 이를 word라고 한다.
- Assembler names
	- \$t0...\$t9 - temporary values
	- \$s0...$s7 - saved variables
#### example
f = (g + h) – (i + j);
f, …, j : $s0, …, $s4
- add $t0, $s1, $s2
- add $t1, $s3, $s4
- sub $s0, $t0, $t1
## Memory 
## Design Principle

- 간단한 것을 위해선 규칙적인 것이 좋다.
	- ex) I-Format, R-Format등이 정해져있음
- 작은 것이 더 빠르다
	- ex) memory도 빠르지만 빠른 계산을 위해 register를 사용한다.
- 