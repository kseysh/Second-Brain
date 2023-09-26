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
- Register
	- 하지만 