![[Pasted image 20250203165910.png|450]]
- 사용자가 정의한 프로시저
- RDBMS에 저장되고 사용되는 프로시저
- 구체적인 하나의 태스크를 수행한다.
- 이외에도 조건문을 통해 분기처리를 하거나 반복문을 수행하거나 에러를 핸들링하거나 에러를 일으키는 등의 다양한 로직을 정의할 수 있다.
## 장점
#### application에 transparent 하다
여러 서버를 운영중이어도 1대의 mysql 서버의 procedure만 변경하여 여러 대의 서버에 영향을 끼칠 수 있다.
#### 네트워크 트래픽을 줄여서 응답속도를 향상시킬 수 있다.
WAS에서 하면 select, insert, update 세 가지를 따로 따로 호출하여 세 번의 three-way handshake가 발생할 수 있지만, stored procedure을 활용하면 한 번에 사용할 수 있다.
=> 그래도 잘 설계해서 WAS에서도 한 번만 호출하도록 쿼리를 잘 짜면 괜찮지 않을까?
#### 여러 서비스에서 재사용 가능하다.
django,  spring, node 이렇게 여러 서버를 사용한다면 각자 코드를 써야하지만 stored procedure을 활용하면 안에 있는 것을 사용하면 되기 때문에 여러 서비스에서 재사용 가능하다
## 단점
#### 유지 관리 보수 비용이 커진다.
WAS, DB 두 개의 위치에서 비즈니스 로직을 관리해야 해서 유지 관리 보수 비용이 커진다.



## VS. [[Stored function]]
![[Pasted image 20250203170249.png]]
Stored Function은 return으로 값을 반환하고
Stored Procedure은 파라미터를 이용해 값을 변경하는 느낌