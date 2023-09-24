## Byte
8 bits = 
2진수 : 00000000 to 11111111
10진수 : 0 to 255
16진수 : 00 to FF
## 16진수
컴퓨터에서는 보통 앞에 0x를 붙이면 16진수로 인식한다.

## 8진수
컴퓨터에서 8진수를 나타내고 싶을 때는 앞에 0을 붙인다.

## s
32-bit에서와 64-bit에서의 long값이 다를 수 있는 것 처럼 사용하는 것에 따라 byte가 달라질 수 있다. 컴퓨터마다 bit 값이 다를 수 있으므로 int32_t, int64_t 같은 고정형 자료형을 쓴다면 범용적인 프로그램을 만들 때 이 것을 쓰는 것이 안전할 수 있다.

## Bit-Level Operations in C (Bitwise)
&( and ), |( or ), ~( not ), ^( Exclusive )
ex)
![[Pasted image 20230924121956.png]]

## Logic Operation in C
&& ( AND ), ||( OR ), !( NOT )
모든 것은 0(False) 또는 1(True)으로 표현된다,
EX)
![[Pasted image 20230924122639.png]]

## Shift Operations in C

### Left shift : x << y
왼쪽으로 shift하게 되면 오른쪽에 빈공간이 생기고, 빈공간에는  0이 채워지게 된다.

### Right shift : x>> y

#### Logical shift
오른쪽으로 shift하게 되면 왼쪽에 빈공간이 생기고, 빈공간에는 0이 채워진다.
#### Arithmetic shift
가장 왼쪽에 있는 bit(최상위 bit)를 복사해서 채워넣는다.

ex) 
![[Pasted image 20230924123338.png]]
![[Pasted image 20230924123258.png]]