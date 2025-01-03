# Lock을 사용한다.
![[Pasted image 20250103232831.png|250]]

## spinlock
![[Pasted image 20250103235737.png|400]]
여기서 TestAndSet는 CPU atomic 명령어이다.
 - 실행 중간에 간섭받거나 중단되지 않는다
 - 같은 메모리 영역에 대해 동시에 실행되지 않는다.
### 특징
- 락이 해제될 때까지 계속 루프를 돌기 때문에 CPU 자원을 계속 소비한다.
- 