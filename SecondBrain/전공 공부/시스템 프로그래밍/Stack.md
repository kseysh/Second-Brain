## Procedure Data Flow
### Register
레지스터에서 Return value는 `%rax`이다.
![[Pasted image 20231102231407.png]]
레지스터는 처음에 6개의 매개변수를 꺼내 놓는다
### Stack
만약 매개변수가 6개가 넘어가면 7개째부터는 stack에 값을 저장한다.
![[Pasted image 20231102231728.png]]
함수를 call할 때 
