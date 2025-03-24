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
multi processor architecture의 장점을 가질 수 있다.

