## Design Principle
- 간단한 것을 위해선 규칙적인 것이 좋다.
	- ex) I-Format, R-Format등이 정해져있음
- 작은 것이 더 빠르다
	- ex) memory도 빠르지만 빠른 계산을 위해 register를 사용한다.
- 자주 생기는 일을 빠르게 하라
	- ex) addi를 이용해 불필요하게 레지스터에 상수를 적재하지 않도록 한다.
- 좋은 설계에는 적당한 절충이 필요하다
	- ex) R-format이 규칙이지만, 규칙을 변형한 I-format도 쓰임
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
Store word to `$t0`
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
Immediate라 I임
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
다른 컴퓨터에서 컴파일된 프로그램이 호환되도록 하는 것은 ISA의 표준화덕분이다.
## Shift Operations
![[Pasted image 20250318162821.png|200]]
- shamt: 어느 정도 shift를 진행할지
- 사실 opcode와 레지스터 두개 상수 하나를 사용할 수 있으므로 I-format이 어울릴 것 같지만, R-format을 사용한다.
	- R-format은 opcode하나만 사용해 사용할 수 있는 operation이 한정되어 있어 최대한 R-format을 사용하기 때문이다.
- `sll` : Shift left logical
	- 왼쪽으로 shift 후 0을 채운다.
	- 2<sup>i</sup>를 곱하는 것
- `srl` : Shift right logical
	- 오른쪽으로 shift 후 0을 채운다.
	- 2<sup>i</sup>로 나뉘는 것
#### example
sll $t2, $s0, 4 
- $t2 = $s0 << 4
srl $t2, $s0, 4
- $t2 = $s0 >> 4
## NOT Operations
MIPS는 NOR을 이용해 NOT Operation을 구현한다.
NOT 연산 = `nor $t0, $t1, $zero`
![[Pasted image 20250318163326.png|300]]
## Conditional Operations
### `beq rs, rt, L1`
- rs == rt라면, L1으로 이동
- I-Format
### `bne rs, rt, L1`
- rs != rt라면, L1으로 이동
- I-Format
### `j L1`
- unconditional jump to L1
- **J-Format**
#### J-Format
![[Pasted image 20250318200508.png|300]]
#### example If
`if (i==j) f = g+h;`
`else f = g-h`
f: $s0
g: $s1
h: $s2
i: $s3
j: $s4
```
	bne $s3, $s4, Else
	add $s0, $s1, $s2
	j Exit
Else: sub $s0, $s1, $s2
Exit ...
```
#### example Loop
`while (save[i] == k) i += 1;`
i: $s3
k: $s5
save: $s6
```
Loop: sll $t1, $s3, 2 (save의 시작 주소를 구하기 위해 4 x i를 shift 연산으로 구함)
	  add $t1, $t1, $s6 (save의 시작 주소와 t1을 더함 = save[i]의 주소)
	  lw $t0, 0($t1)
	  bne $t0, $s5, Exit
	  addi $s3, $s3, 1
	   j Loop
Exit: ...
```
## Basic Blocks
embedded branch가 없고, branch의 target이 아닌 순차적인 명령어
명령어가 sequential하게 실행되는 것을 보장하는 곳에서 주로 최적화를 수행한다
(다른 곳에서 jump해서 오거나, 나가면 최적화 범위가 너무 넓어지기 때문)
## More Conditional Operations
eq, ne는 branch로 구성되고, lt, gt, le, ge는 set으로 구성된다.
=> set: 작으면 특정 레지스터 값을 1로 만들어라
### `slt rd, rs, rt`
- rs < rs라면, rd를 1로 만들고 아니면 0으로 만들어라
- R-format
### `slti rt, rs, constant`
- rs < constant라면, rt를 1로 만들고 아니면 0으로 만들어라
- I-format
#### example
if ($s1 < $s2)
```
slt $t0, $s1, $s2
bne $t0, $zero, L
```
#### signed vs. Unsigned example
뒤에 u가 들어가면 unsigned로 계산
c++에서 unsigned 계산을 하면 뒤에 u를 붙여서 계산한다.
`$s0 = 1111 1111 1111 1111`
`$s1 = 0000 0000 0000 0001`
`slt $t0, $s0, $s1` => `$t0 = 1`
`sltu $t0, $s0, $s1` => `$t0 = 0`
### Branch Instruction Design
- 왜 blt, bge를 만들지 않았을까?
- 하드웨어에서는 equal 비교보다 대소비교가 더 느리기 때문
- 컴퓨터는 한 clock마다 한 instruction을 수행하는 것을 기본으로 한다.
- 하지만 blt로 인해 느린 동작을 하면 clock의 단위를 늘릴 수 밖에 없고 이는 컴퓨터 성능을 낮출 수 있기 때문이다.
- 따라서, set, branch의 두 가지 동작으로 분리하였다.
## Procedure Calling step
Procedure = 함수
1. 매개변수를 레지스터에 배치
2. 제어를 프로시저로 전달 (지금 실행중인 코드 흐름을 멈추고, 새로운 함수를 점프해서 실행한다)
3. 프로시저를 위한 저장 공간 확보
4. 프로시저 연산 수행
5. 호출 함수를 위해 결과를 레지스터에 저장
6. 호출한 위치로 복귀
이처럼 함수 호출 전후에 데이터를 주고받아야 할 필요가 있기 때문에, 레지스터의 일부를 argument 전달과 return 값 전달로 나누어 두었다.

함수 호출 전
- `$a0 ~ $a3`에 필요한 파라미터 배치
함수 return 전
- return 값을 `$v0~$v1`에 배치
## Register Usage
- $a0 – $a3: arguments
	- caller-saved
- $v0, $v1: result values
	- caller-saved
- $t0 – $t9: temporaries
	- caller가 백업해둘 의무를 가짐
	- caller-saved register
- $s0 – $s7: saved
	- callee가 사용하고 다시 원상복구할 책임을 가짐
	- *callee*-saved register
- $gp: global pointer for static data
- $sp: stack pointer
	- stack 공간을 어디까지 사용하는지에 대한 포인터
	- stack 공간은 위에서 아래로 내려오므로, sp는 스택의 최하위 주소를 가짐
- $fp: frame pointer
- $ra: return address 
	- caller-saved
## Procedure Call Instructions
컨트롤을 넘겨주는 행위 
### `jal ProcedureLabel` (J-Format)
- Jump And Link
- 주어진 Label로 Jump를 하는데, 이 instruction 이후에 실행되는 instruction의 주소를 `$ra`에 넣어둔다. (함수를 마치고 돌아와야 할 주소)
### `jr rs` (R-Format)
- Jump Register
- jr $ra처럼 쓰는데, ra에 적힌 주소로 PC를 옮긴다.
	- PC = 다음번에 수행해야 하는 주소를 가짐
#### example
###### Leaf Procedure (다른 함수 호출을 하지 않는 함수)
```c
int leaf_example (int g, h, i, j) { // $a0, ... , $a3에 저장됨
	int f; // $s0에 저장됨, callee가 사용하고 다시 원상복구 해두어야 한다.
	f = (g + h) - (i + j);
	return f; // $v0에 저장됨
}
```

```c
leaf_example:
	addi $sp, $sp, -4 // 스택 포인터를 4 빼줌 ($s0가 4byte이므로)
	sw $s0, 0($sp) // $s0를 stack에 저장
	add $t0, $a0, $a1
	add $t1, $a2, $a3
	sub $s0, $t0, $t1 // procedure body
	add $v0, $s0, $zero // Result
	lw $s0, 0($sp) 
	addi $sp, $sp, 4 // Restore $s0 
	jr $ra // Return
```
###### Non-Leaf Procedure (다른 함수를 호출하는 함수)
nested call에서는, caller는 return address, arg, temp를 stack에 저장해두고, 호출 이후에 복구한다.
```c
int fact (int n) { // $a0에 저장됨
	if (n < 1) return 1;
	else return n * fact(n - 1); // Result는 $v0에 저장됨
}
```

```
fact:   slti $t0 $a0 1
		beq $t0 $zero else // callee saved지만, 다시 사용할 일 없으므로 save하지 않아도 됨
		addi $v0 $zero 1
		jr $ra
else:   addi $sp $sp -8
		sw $ra 4($sp) // 나는 call하기 직전 save 해놓는게 좋은 듯
		sw $a0 0($sp) // n은 fact(n-1)과 곱할 때 사용해야 하니까 save 해둠
		add $a0 $a0 -1
		jal fact
		lw $a0 0($sp)
		lw $ra 4($sp)
		addi $sp $sp 8
		mul $v0 $v0 $a0
		jr $ra
```
saved value가 있었으면, 호출 받았을 때 계속 saved value를 저장해두고 리턴시 복구했어야 했을 것임

n=2로 시작한다는 가정
ra와 a0는 stack에 저장됨
![[Pasted image 20250324174324.png|300]]
![[Pasted image 20250324174336.png|300]]
![[Pasted image 20250324174348.png|300]]
jal을 하면서 $ra가 L1 + 8로 변한다
![[Pasted image 20250324174402.png|300]]
![[Pasted image 20250324174415.png|300]]
![[Pasted image 20250324174428.png|300]]
![[Pasted image 20250324174438.png|300]]
return 1을 하고, 위에서 확보한 stack pointer를 반납한다.
![[Pasted image 20250324174453.png|300]]
stack에 저장되어 있던 N-1을 $a0로 복구시킨다.
stack에 저장되어 있던 주소를 $ra로 복구시킨다.
sp를 올려준다.
![[Pasted image 20250324174508.png|300]]
![[Pasted image 20250324174523.png|300]]
![[Pasted image 20250324174536.png|300]]
## frame pointer
![[Pasted image 20250324175720.png|300]]
$fp(frame pointer): 현재 함수의 최초 stack top을 기록해두는 것
return하기 전에는 유지되는 값
sp는 계속 변해서 tracking하기가 조금 번거롭지만, 고정되어 있는 fp를 사용해서 컴파일러가 위치 계산이 좀 더 편하도록 한다.
## Memory Layout
$gp: 전역변수를 관리하는 공간의 중간주소
$gp + N, $gp - N의 형태로 전역변수를 저장해둔다.
## Byte/Halfword Operations
### `lb rt, offset(rs), lh rt, offset(rs)`
rs + offset 주소에서 1 byte를 읽어서 rt에 저장
1byte만을 읽어 word size인 4 byte에 저장해야 하므로 sign extend를 진행한다.
### `lbu rt, offset(rs), lhu rt, offset(rs)`
unsigned이므로, zero extend를 진행한다.
### `sb rt, offset(rs), sh rt, offset(rs)`
가장 오른쪽의 byte나 halfword를 저장한다.
따라서 sbu shu 이런게 없다.
### example
```cpp
void strcpy (char x[], char y[])
{
	int i;
	i = 0;
	while ((x[i]=y[i])!='\0'){
		i += 1;
	}
}
```
x = $a0
y = $a1
i = $s0
![[Pasted image 20250325142713.png|400]]
굳이 lbu가 아니어도 되긴 함
![[Pasted image 20250413230830.png|200]]
load한 value와 주소 값 구분 잘 하기
## 32-bit Constants
대부분의 상수는 작아서 16bit로 충분하지만, 그보다 크다면 추가적인 방법이 필요하다.
### `lui rt constant`
load upper immediate
- 16 bit 상수를 왼쪽으로 16 bit만큼 옮기고, 나머지를 0으로 채운다.
- 이후 or 연산을 적용하면 32bit를 가지는 레지스터를 상수처럼 사용할 수 있다
or immediate
![[Pasted image 20250325143017.png|400]]
## Branch Addressing (beq, bne)
![[Pasted image 20250325143558.png|300]]
**PC-relative addressing**을 사용한다.
- 현재 PC에서 얼만큼 떨어져 있는지를 사용한다 (대부분의 branch는 비슷한 code 공간에 있기 때문)
- Target address = **현재 PC + offset x 4**
- 따라서 address에는 18bit까지 표현할 수 있다.
- instruction의 크기는 4byte이므로, 4의 배수가 아닌 값은 쓸모가 없다.
	- 따라서 constant address는 뒤에 00이 생략되어 있다.
- 실제 주소 크기는 word size인 32bit이다.
## Jump Addressing (j, jal)
Jump는 text segment에서 어디든 타겟할 수 있다.
![[Pasted image 20250325143928.png|400]]
**(Pseudo)Direct jump addressing**을 사용한다. (J-Format은 address 부분이 크기 때문)
- Target address = **PC<sub>31...28</sub> + address x 4**
- 여기에서도 Branch Addressing과 같은 이유로 address에 4를 나눠 저장해둔다.
- 표현은 28bit까지 할 수 있는데 target 주소는 32bit이다.
	- 따라서, 4bit는 현재 PC의 상위 4bit로 채운다.
### **example**
PC는 항상 다음 instruction의 주소를 가리킴
항상 주소는 4를 곱해서 이해해야 함
![[Pasted image 20250325144436.png|400]]
### Branch가 16bit offset으로 표현하기 너무 크다면,
assembler가 code를 변경한다.
`beq $s0, $s1, L1`
=>
```
bne $s0, $s1, L2
j L1
L2: ....
```
18bit로 표현 불가능하기 때문에 j로 표현한다.
## Addressing Mode Summary
![[Pasted image 20250325150307.png|300]]
## Assembler Pseudo-instructions
대부분의 assembler instructino은 기계어와 1대1 대응이지만, Pseudo-instruction은 assembler가 변형해서 만들기도 한다.
실제로 기계어에 없던 instruction을 허용하는 instruction
`move $t0, $t1` 
=> 
`add $t0, $zero, $t1`

`blt $t0, $t1, L`
=>
```
slt $at, $t0, $t1
bne $at, $zero, L
```

$at: assembler temporary
- 어셈블러가 어셈블리어를 기계어로 번역하면서 임시 레지스터가 필요할 때 사용되는 레지스터
## Starting Java Applications
![[Pasted image 20250327152038.png|300]]
## C Sort Example
### Swap 함수
```c
void swap(int v[], int k)
{
	int temp;
	temp = v[k];
	v[k] = v[k+1];
	v[k+1] = temp;
}
```
v: $a0
k: $a1
temp: $t0
![[Pasted image 20250327152149.png|300]]
따로 k+1을 관리할 필요가 없다.
### Sort Procedure
```c
void sort (int v[], int n)
{
	int i, j;
	for (i = 0; i < n; i += 1) {
		for (j = i– 1; j >= 0 && v[j] > v[j + 1]; j -= 1) {
			swap(v,j);
		}
	}
}
```

![[Pasted image 20250327152351.png|350]]
![[Pasted image 20250327152337.png|400]]
![[Pasted image 20250327152413.png|350]]
![[Pasted image 20250414113811.png]]
나름 잘 한듯..?

## Effect of Compiler Optimization / Language and Algorithm
- Instruction count와 CPI는 단독으로 좋은 성능 지표가 아니다.
	- (Instruction count가 높은대신 CPI가 높을 수 있기 때문)
- 컴파일러 최적화는 알고리즘에 민감하다
- Java/JIT로 컴파일된 코드는 JVM intertpret보다 훨씬 빠르다
	- 어떤 경우에는 최적화된 C와 비슷하다.
## Arrays vs. Pointers
- 배열 인덱싱
	- 변수 크기로 index multiplication
	- array base address에 더하기
- 포인터
	- 메모리 주소에 직접 대응
	- 인덱싱 복잡성을 피할 수 있음

"강도 감소"를 시프트로 곱하기 
배열 버전은 시프트가 내부 루프에 있어야 합니다 • 
	증가된 i에 대한 인덱스 계산의 일부 • 
	c.f. 증가 포인터 • 
컴파일러는 포인터의 수동 사용과 동일한 효과를 얻을 수 있습니다 • 
	유도 변수 제거 • 
	프로그램을 더 명확하고 안전하게 만드는 것이 좋습니다
### example
![[Pasted image 20250327153020.png|400]]

arrays방식은 shift가 내부 loop에 있어야 한다.
index로 부터 주소를 계산하는 과정이 계속 필요하다.
## ARM VS. MIPS
ARM은 MIPS보다 Registers의 수를 줄였다.
ARM은 MIPS보다 Data addressing mode의 수가 더 많다.
ARM은 word size를 64bit로 늘렸다.
## Fallacies (오류들)
- 강력한 명령어가 곧 높은 성능을 의미하지는 않음
	- 명령어 수가 무조건 적다고 좋지는 않다.
		- 단순한 명령어를 많이 쓰는 것이 더 유리할 수 있다.
	- 복잡한 명령어는 구현이 어렵다
		- 복잡한 명령어는 간단한 명령어를 포함한 전체 실행 속도를 느리게 할 수 있다.
	- 컴파일러는 단순한 명령어로도 빠른 코드를 잘 생성한다.
- 고성능을 위해 어셈블리 코드를 사용할 필요는 없다
- Instruction set은 호환성을 위해 사라지지 않고 점점 추가된다.
## 주의사항
- 연속된 단어들이 반드시 연속된 메모리 주소에 저장되지 않는다.
- 메모리 접근시 word 단위인 4씩 증가시켜야 한다
ex)
```cpp
int* func(){
	int a = 1;
	return &a; // 이상한 값이 들어가 있을 수 있음
}
```