시험에는 X
## 포인터
어떤 값의 주소를 저장하는 변수
변수 앞에 \*을 붙이면 주소를 저장하겠다 라는 의미
선언할 때 \*을 붙이면 앞의 값이 주소를 저장하고 있다는 의미
ex) int** i; 라면 i 앞에 \*이 있으므로 i에는 int\*를 저장하고 있는 변수가 있다.
포인터가 주소를 저장해기 때문에 값을 확인할 때, 주소에 도착해서 int\*이라면 int 사이즈만큼의 값을 가져와서 데이터를 참조한.
### 선언할 때와 사용할 때 \*의 의미의 차이점
- 선언할 때
	- 앞의 값이 주소를 저장하고 있다
- 사용할 때
	- 그 주소를 따라가서 값을 보겠다.
### Reference &
이 변수가 가지고 있는 주소를 보겠다.

## Call by Value vs. Call by Reference
[[Call by value]]
[[Call by reference]]
> C는 Call by value 언어이다.


## Pointer Arithmetic
포인터는 계산시에 변수의 크기에 맞게 연산된다.
![[Pasted image 20230922222810.png]]
int는 4byte
char는 1byte
주소는 8byte

## Function Pointer
C에서, 함수도 포인터로 저장되어 있는 값이며, 주소로 접근할 수 있다.
## Struct
서로 다른 자료형을 연속된 공간에 묶어놓은 것
같은 자료형을 연속된 공간에 묶어놓고 싶다면, array를 사용하면 된다.

접근 시에 .이나 ->로 접근한다.

String은 끝이 '\\0'으로 끝나는 char Array이다.

## heap에 값이 할당될 때 (동적 할당)
## Malloc
c의 new와 대응
## Free
c의 delete와 대응
## Calloc
