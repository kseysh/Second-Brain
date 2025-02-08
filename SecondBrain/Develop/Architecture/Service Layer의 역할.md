[참고](https://sandcastle.tistory.com/m/84)
# Service Layer의 역할
개잘하고자 하는 서비스의 핵심적인 비즈니스 로직들이 모여있는 계층이다.
> 비즈니스 로직?
> 상세 구현 로직은 잘 모르더라도 비즈니스의 흐름을 이해할 수 있는 로직

### Service 레이어에서 비즈니스와 구체적인 구현 부분을 합쳐서 생각하지 말자!
`컨트롤러` -> `서비스` -> `레포지토리` 3계층으로 생각하는 것이 아닌,
`컨트롤러` -> `서비스` -> `구현` -> `레포지토리` 의 4계층 체제로 생각할 필요가 있다.

이를 각각의 역할로 정의해보자면,
`Presentation Layer` -> `Business Layer` -> `Implementation Layer` -> `Data Access Layer`  
로 정의할 수 있다.

#### 예제
##### AS-IS
![[Pasted image 20240505161502.png]]
##### TO-BE
![[Pasted image 20240505161442.png]]

## Layer의 규칙
![[Pasted image 20240505162704.png]]
![[Pasted image 20240505162718.png]]
![[Pasted image 20240505162748.png]]
![[Pasted image 20240505162835.png]]
