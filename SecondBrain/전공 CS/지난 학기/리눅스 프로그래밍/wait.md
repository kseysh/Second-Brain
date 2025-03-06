## 자식 프로세스와의 동기화
- 프로세스가 종료되면 (정상적이든 비정상적이든)
  - 커널은 프로세스의 모든 열린 디스크립터를 닫고, 사용 중이던 메모리를 해제합니다.
  - 커널은 종료된 프로세스에 대한 proc entry만 유지합니다 (pid, 종료 상태 - exit(status), CPU 시간).
  - 이 정보는 종료된 프로세스의 부모 프로세스가 확인할 때까지 남아 있습니다.
  - 부모 프로세스는 wait() 시스템 호출을 통해 이 정보를 확인합니다.
  - wait() 후에 종료된 프로세스의 proc entry는 proc 테이블에서 해제됩니다.
## wait(2) 시스템 호출
```c
#include <sys/wait.h>
pid_t wait(int *statloc);
// Return: child process ID if OK, 0 (see later), or -1 on error
```
- wait는 자식 프로세스가 실행되는 동안 프로세스의 실행을 일시 중지합니다.
- 자식이 종료되면 대기 중인 부모 프로세스가 다시 시작됩니다.
- 여러 자식 프로세스가 실행 중인 경우, wait는 부모의 자식 중 하나가 종료되면 반환됩니다.
- wait가 (pid_t) -1을 반환하면, 자식이 없음을 의미할 수 있습니다.
  - errno = ECHILD

child가 여러개면 wait도 여러개이어야 한다.
wait(NULL)은 stat값을 받지 않겠다는 의미, -1이 반환될 때까지 wait한다.

## waitpid(2) 시스템 호출
```c
#include <sys/wait.h>
pid_t waitpid(pid_t pid, int *statloc, int options);
// Return: child process ID if OK, 0 (see later), or -1 on error
```
wait할 pid를 지정하는 system call
- 언급했듯이, 여러 자식이 있는 경우 wait는 자식 중 하나가 종료될 때 반환됩니다.

pid == -1 모든 자식 프로세스를 기다립니다. 이 점에서 waitpid는 wait와 동일합니다.
pid > 0 프로세스 ID가 pid와 동일한 자식을 기다립니다.
pid == 0 호출 프로세스의 프로세스 그룹 ID와 동일한 프로세스 그룹 ID를 가진 모든 자식을 기다립니다.
pid < 0 프로세스 그룹 ID가 pid의 절대값과 동일한 모든 자식을 기다립니다.

`WNOHANG`: pid로 지정된 자식이 즉시 사용 가능하지 않으면 waitpid 함수는 블록하지 않습니다. 
이 경우 반환 값은 0입니다. (hang = 매달다 = 기다리다 NOHANG = 기다리지 말라)
## 자식 프로세스 종료 상태 확인
`WIFEXITED(status)`, `WEXITSTATUS(status)`
자식 프로세스가 정상적으로 종료되었을 때 0이 아닌 값을 반환합니다. WIFEXITED가 0이 아닌 값을 반환하면, WEXITSTATUS는 자식 프로세스가 `_exit()`, `exit()` 또는 main 함수에서 반환한 하위 8비트 값을 반환합니다.

`WIFSIGNALED(status)`,`WTERMSIG(status)`,`WCOREDUMP(status)` 
자식 프로세스가 캐치되지 않은 시그널로 인해 종료되었을 때 0이 아닌 값을 반환합니다. WIFSIGNALED가 0이 아닌 값을 반환하면, WTERMSIG는 종료를 유발한 시그널 번호를 반환합니다.

`WIFSTOPPED(status)`, `WSTOPSIG(status)` 
자식 프로세스가 현재 중지된 경우 0이 아닌 값을 반환합니다. WIFSTOPPED가 0이 아닌 값을 반환하면, WSTOPSIG는 자식 프로세스가 중지된 시그널 번호를 반환합니다.

`WIFCONTINUED(status)` 
자식 프로세스가 현재 다시 실행 중인 경우 0이 아닌 값을 반환합니다.

# 좀비프로세스와 조기 종료

## 좀비 프로세스
- 부모가 종료된 자식에 대해 wait()를 호출하지 않으면 좀비 상태가 됩니다 (ps에서 "Z" 상태로 표시됨).
- 부모가 wait() 또는 waitpid()를 호출할 때까지 좀비는 남아 있습니다.
- 이는 실행을 완료했지만 프로세스 테이블에 항목이 남아 있는 프로세스입니다.
![[Pasted image 20241013164619.png|300]]
## 고아 프로세스
- 부모 프로세스가 죽으면 자식 프로세스는 고아 프로세스가 됩니다.
- init 프로세스 (pid = 1)가 고아 프로세스의 부모가 됩니다.
- init 프로세스는 주기적으로 자식에 대해 wait를 실행하므로 결국 고아 좀비는 제거됩니다.

![[Pasted image 20241013164721.png|300]]