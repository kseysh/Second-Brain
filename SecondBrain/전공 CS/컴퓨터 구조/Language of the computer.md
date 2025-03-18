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
## Memory Operands
Memory는 byte 단위로 addressing된다.
Word는 메모리내에 정렬되어 있다. 따라서 word의 주소는 항상 4(word단위인 4byte)의 배수이다
MIPS는 Big Endian을 사용한다.
### Little vs. Big endian
![[Pasted image 20250318151919.png|400]]
### `lw $t0, 8($s1)`
Load word to `$t0`
dest src 순서
### `sw $t0, 8($s1)`
Load word to `$t0`
dest src 순서
#### example 1
g = h + A\[8]
g : `$s1`
h : `$s2`
A : `$s3`
- `lw $t0, 32($s3)`
	- A\[0]부터 4byte x 8만큼 떨어져 있으므로 32($s3)를 load하는 것
- `add $s1, $s2, $t0`
#### example 2
A\[12] = h + A\[8]
h : `$s2`
A : `$s3`
- `lw $t0, 32($s3)`
- `add $t0, $s2, $t0`
- `sw $t0, 48($s3)`
## Registers vs. Memory
레지스터가 메모리보다 빠름
따라서 컴파일러는 최대한 레지스터에 자주 사용되는 데이터를 적재하여 optimization을 해야한다.
## Immediate Operands
### `addi $s3, $s3, 4`
상수를 더할 때 사용
subi는 존재하지 않고, 대신 음수 constant를 사용한다.
## `$zero`
0은 자주 사용하는 상수라서 0번 레지스터에 $zero 레지스터를 지정한다.
레지스터간 데이터를 이동할 때, add와 $zero를 이용한다.
add $t2, $s1, $zero -> $s1에 있는 데이터를 $t2로 옮김
## 2s-Complement Signed Integers
MSB가 1이면, 모든 bit에 not 연산(Complement) 후, 1을 더하면 된다.
음수가 양수보다 절댓값이 1 더 크다
## Sign Extension
- addi - Immediate Value를 레지스터에 더할 때, 16bit->32bit로 확장함
- lb, lh - 메모리에서 load Byte(1 byte load), Load Halfword(2 byte load)를 할 때, 32bit 레지스터에 저장하기 위해 부호 확장을 진행함
- beq, bne - Branch 명령어에서 Displacement offset은 16bit이므로, 32bit로 확장해야함
부호를 유지하기 위해 가장 왼쪽 비트를 복제한다.
ex) 
+2: 0000 0010 => 0000 0000 0000 0010
-2: 1111 1110 => 1111 1111 1111 1110
## R-format Instructions
![[Pasted image 20250318161034.png|300]]
- op - operation code
- rs - first source register number
- rt - second source register number
- rd - destination register number
- shamt - shift amount
- funct - function code (extends opcode)
#### example
![[Pasted image 20250318161150.png|300]]
## I-format Instructions
![[Pasted image 20250318161318.png|300]]
- rt - destination or source register number
- rs - source register number
- constant - -2<sup>15</sup> ~ 2<sup>15</sup>-1
- address - address offset
#### example
A\[300] = h + A\[300]
- `lw $t0, 1200($t1)`
- `add $t0, $s2, $t0`
- `sw $t0, 1200($t1)`
$t0: 8
$t1: 9
$t2: 18
![[Pasted image 20250318162100.png|200]]
## Stored Program Computers


## Design Principle
- 간단한 것을 위해선 규칙적인 것이 좋다.
	- ex) I-Format, R-Format등이 정해져있음
- 작은 것이 더 빠르다
	- ex) memory도 빠르지만 빠른 계산을 위해 register를 사용한다.
- 자주 생기는 일을 빠르게 하라
	- ex) addi를 이용해 불필요하게 레지스터에 상수를 적재하지 않도록 한다.
- 좋은 설계에는 적당한 절충이 필요하다
	- ex) R-format이 규칙이지만, 규칙을 변형한 I-format도 쓰임