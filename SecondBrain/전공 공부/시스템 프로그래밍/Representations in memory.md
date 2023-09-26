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

## Representing Pointers
같은 프로그램을 돌리더라도 포인터 값이 다를 수 있다.
ex)
![[Pasted image 20230926113318.png]]
P의 값은 돌릴 때 마다 달라질 수 있다.

## Representing Strings
String은 Char의 Array이다.
string은 마지막에 null character인 \\0으로 끝난다.
String은 Byte Ordering에 관계없다.
Array는 첫 번째 주소 이후에 차곡차곡 쌓이는 형태이기 때문 (int는 4byte가 하나의 완결형 데이터이므로 상관있는 것)
String은 내부에 순서가 있는 형태라 상관이 없다.
![[Pasted image 20230926113649.png]]

ex)
![[Pasted image 20230926114329.png]]
 코드 해석 : 
 ( index << 3 ) == ( index \* 8 )
 따라서 원하는 integer만큼의 4byte를 오른쪽에 붙여놓고, 0xFF를 통해 오른쪽에 붙여져있던 8-bit integer만 가지고 오는 함수
 코드에서 잘못된 점 : unsigned의 계산이므로 오른쪽으로 shift를 진행하면 왼쪽에는 0
 integer가 음수이게 되면 오른쪽에 붙여놨을 때,