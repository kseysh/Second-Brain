## Procedure Data Flow
### Register
레지스터에서 Return value는 `%rax`이다.
![[Pasted image 20231102231407.png]]
레지스터는 처음에 6개의 매개변수를 꺼내 놓는다
### Stack
만약 매개변수가 6개가 넘어가면 7개째부터는 stack에 값을 저장한다.
![[Pasted image 20231102231728.png]]
함수를 call할 때 Return address는 stack의 top에 있고, 나머지 매개변수들은 그보다 아래에 있다.

