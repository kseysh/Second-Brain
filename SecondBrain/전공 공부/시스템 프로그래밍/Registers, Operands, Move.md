# Registers
## x86-64 Integer Registers
![[Pasted image 20230926195028.png]]
각 레지스터는 8byte (64bit)이고 16개이다.
각 레지스터는 절반만 접근도 가능하며 이름을 달리하여 접근하면 된다. 하위 4/2/1 byte도 접근이 가능하다.
ex)
%rax + % rbx => 64bit + 64bit
%eax + %ebx => 32bit + 32bit
### %rsp
Stack Pointer로, 용도가 고정되어 있음

# Moving Data
`movq Source Dest`
뜻 : Source에 있는 것을 Dest로 옮긴다.
mov가 명령어 q는 접미사다
접미사의 종류로는 
q (quad) : 64bit
l (long) : 32bit
w (word) : 16bit
b (byte) : 8bit
> 하지만 수업에서는 복잡성을 없애고자 64bit를 기준으로 설명!
> qlwb는 몰라도...될 듯?

## Operand types
Source, Dest자리에 올 수 있는 것들
- Register
	- 하지만 %rsp는 용도가 고정되어 있어 사용할 수 없다.
	- ex) 
- Memory
	- 보통 address가 주어짐
	- address는 어떤 register에 들어있음.
	- ex) (%rax) => 이 것처럼 괄호를 붙여 레지스터 이름을 써주면 %rax를 옮기겠다는 뜻이 아닌 %rax안에 들어있는 주소에서 어떤 값을 빼거나 넣겠다는 의미
- immediate(상수)
	- $ 표시를 붙여준다.
	- 상수의 크기에 따라서 1,2,4 byte를 알아서 encoding해준다.
	- ex) $0x400 => 16진수인 400 , $-533 => 10진수인 -533

## `movq` Operand Combinations
![[Pasted image 20230927015011.png]]

