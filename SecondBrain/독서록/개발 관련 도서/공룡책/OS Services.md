## OS View
![[Pasted image 20250311165258.png|400]]
## 유저에게 도움이 되는 함수
- User interface
- Program execution
- I/O operations
- File-system manipulation
- Communications
- Error detection
## 리소스 공유를 통한 효율적인 동작 보장
- Resourece allocation
- Accounting
- Protection and security
## System call
![[Pasted image 20250311170610.png|200]]
system call 호출시 kernel의 특정 테이블에서 instruction의 주소를 찾고 그것을 실행한다.
=> system call 요청은 user level에서 하는 것이기 때문에
## User mode vs. Kernel mode
일부 instruction은 커널에서만 수행이 가능하다. (유저가 중요한 데이터들을 훼손하면 안되므로)
따라서 cpu가 processor status register를 가지고 누가 instruction을 실행하는지 판단한다.
- kernel mode: 0
- user mode: 1
