# Byte addressing memory
메모리는 책장과 같고 책장 한 에는 1byte가 들어갈 수 있다.
## pointer
책장 한 칸(1 byte)의 주소
## word size
주소에 필요한 사이즈
컴퓨터의 32bit, 64bit가 word size임. 요즘 컴퓨터는 64bits의 word size를 가지고 있음.

![[Pasted image 20230926111230.png]]

## Byte Ordering
Endian : "끝" 이라는 의미
### Big Endian
끝이 큰 수라는 의미
소형 컴퓨터/임베디드 시스템에서 주로 사용
장점 : 사람이 읽는 순서대로 읽혀 주소를 직접 바꾸거나 할 때 유리하다.
### Little Endian
끝이 작은 수라는 의미
보통의 컴퓨터가 Little Endian을 사용
장점 : Truncating이나 Expansion이 좀 더 자유롭다.
=> 주소의 시작부분에 최하위 비트(least significant bit)가 있기 때문에
=> casting시 별도의 작업을 해주지 않아도 된다.

### Byte Ordering Example

16진수이고, 변수 x가 4byte 이므로 01,23,45,67 로 Byte Ordering이 될 것이다. 
![[Pasted image 20230926111709.png]]
Big Endian : 큰 주소로 끝내야 하므로, 0x100부터 넣기 시작하여  0x103까지 넣는다.
Little Endian : 작은 주소로 끝내야 하므로, 0x103부터 넣기 시작하여  0x100까지 넣는다.