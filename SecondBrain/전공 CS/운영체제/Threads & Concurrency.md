## 멀티스레드의 장점
#### Responsiveness
프로세스의 일부가 Block되어도 계속 실행될 수 있다.
#### 자원 공유
스레드는 프로세스의 자원을 공유하며, shared memory나 메시지 전달 방식보다 더 쉽게 사용할 수 있다.
IPC가 필요하지 않다.
#### 경제적
- 프로세스 생성/종료보다 시간이 적게 든다
- 같은 프로세스의 thread간 변경 비용이 적다.
- Stack 과 스레드 당 static memory의 적은 리소스를 사용한다.
#### 확장성
multiprocessor(CPU 코어가 여러개) architecture의 장점을 가질 수 있다.
# Multithreaded Process Models
멀티스레드는 고려할 사항이 많아진다.
- Job 분배
- Data 분리
- Data dependency
## Parallelism
물리적으로 동일한 시간에 작동하는 것
![[Pasted image 20250324203703.png|300]]
### Types of parallelism
- Data parallelism - 데이터를 나누는 것 (하나의 일, 다른 데이터)
- Task parallelism - 일을 나누는 것 (서로 다른 일)
## Concurrency
동일한 시간에 작동하는 "것"처럼 보이는 것
![[Pasted image 20250324203651.png|300]]
## Traditional Process Model
- process