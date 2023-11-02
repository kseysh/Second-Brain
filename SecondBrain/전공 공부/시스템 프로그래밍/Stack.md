## Procedure Data Flow
### Register
레지스터에서 Return value는 `%rax`이다.
![[Pasted image 20231102231407.png]]
레지스터는 처음에 6개의 매개변수를 꺼내 놓는다
`%rdi`
`%rsi`
`%rdx`
정도는 기억하면 좋다

### Stack
만약 매개변수가 6개가 넘어가면 7개째부터는 stack에 값을 저장한다.
![[Pasted image 20231102231728.png]]
함수를 call할 때 Return address는 stack의 top에 있고, 나머지 매개변수들은 그보다 아래에 있다.

### 예시
![[Pasted image 20231103000940.png]]
mult2를 호출할  순서대로 rdi는 x, rsi는 y에 배치된다.
리턴 값은 반드시 rax에 들어간다.

스택의 top에는 `%rsp`에 return address 적혀있고, 그 한 칸 위(+8)에는 a7이 적혀있다.
r