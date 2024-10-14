# exit 시스템 호출로 프로세스 종료

## 프로세스 종료
- 프로세스는 여덟 가지 방법으로 종료될 수 있습니다.
### 정상 종료
  - main 함수에서 반환
  - exit 호출
  - \_exit 또는 \_Exit 호출
### 비정상 종료
  - abort 호출
  - signal 수신
  - 마지막 스레드가 취소 요청에 응답
## exit(3) 시스템 호출
- exit의 인수는 프로세스의 종료 상태를 나타냅니다.
- 관례적으로, 프로세스는 다음을 반환합니다:
  - 정상 종료 시 0
  - 문제가 발생하면 0이 아닌 값
- `_exit`와 `_Exit`는 즉시 커널로 반환됩니다.
- exit는 특정 정리 작업을 수행합니다.
  - 표준 I/O 라이브러리의 깨끗한 종료 (fclose)
- exit(0)은 main()에서 return(0)과 동일합니다.
![[Pasted image 20241012221856.png|150]]
## `atexit(3)`
```c
#include <stdlib.h>
int atexit(void (*func)(void));
// Returns: 0 if OK, nonzero on error
```
- ISO C에서는 프로세스가 exit에 의해 자동으로 호출되는 최대 32개의 함수를 등록할 수 있습니다.
- 이러한 함수들은 exit 핸들러라고 불리며 atexit 함수를 호출하여 등록됩니다.
- exit 함수는 이 함수들을 등록된 역순으로 호출합니다.
- 플랫폼에 의해 지원되는 최대 exit 핸들러 수
![[Pasted image 20241012222342.png|500]]
exit은 하나만 exit해주는 것이 아니라 여러가지를 exit해주고 돌아온다.
`_exit``_Exit`은 cleanup 작업을 안하고 바로 kernel에 exit 작업을 해준다.
![[Pasted image 20241012222602.png|600]]
# 프로세스 동기화

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
- 부모 프로세스는 자식 중 하나가 종료될 때까지 루프에서 기다릴 수 있습니다.
![[Pasted image 20241013160702.png|400]]
child가 여러개면 wait도 여러개
wait(NULL)은 stat값을 받지 않겠다는 의미, -1이 반환될 때까지 wait한다.
![[Pasted image 20241013160933.png|500]]
![[Pasted image 20241013161022.png|500]]
보통 status 정보는 exit을 통해 자식의 exit-status가 된다.
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

`WNOHANG`: pid로 지정된 자식이 즉시 사용 가능하지 않으면 waitpid 함수는 블록하지 않습니다. 이 경우 반환 값은 0입니다. (hang = 매달다 = 기다리다 NOHANG = 기다리지 말라)
![[Pasted image 20241013162935.png|600]]
Still waiting은 4번 print 할 것
## wait(2) & waitpid(2) 함수의 차이점
  - wait 함수는 호출자를 자식 프로세스가 종료될 때까지 블록할 수 있습니다. 반면에 waitpid는 블록을 방지하는 옵션이 있습니다 (`WNOHANG`).
  - waitpid 함수는 종료된 자식을 기다리지 않으며, 어떤 프로세스를 기다릴지 제어하는 여러 옵션이 있습니다.
## 종료 상태 확인 (2/2)
`WIFEXITED(status)`, `WEXITSTATUS(status)`
자식 프로세스가 정상적으로 종료되었을 때 0이 아닌 값을 반환합니다. WIFEXITED가 0이 아닌 값을 반환하면, WEXITSTATUS는 자식 프로세스가 _exit(), exit() 또는 main 함수에서 반환한 하위 8비트 값을 반환합니다.

`WIFSIGNALED(status)`,`WTERMSIG(status)`,`WCOREDUMP(status)` 
자식 프로세스가 캐치되지 않은 시그널로 인해 종료되었을 때 0이 아닌 값을 반환합니다. WIFSIGNALED가 0이 아닌 값을 반환하면, WTERMSIG는 종료를 유발한 시그널 번호를 반환합니다.

`WIFSTOPPED(status)`, `WSTOPSIG(status)` 
자식 프로세스가 현재 중지된 경우 0이 아닌 값을 반환합니다. WIFSTOPPED가 0이 아닌 값을 반환하면, WSTOPSIG는 자식 프로세스가 중지된 시그널 번호를 반환합니다.

`WIFCONTINUED(status)` 
자식 프로세스가 현재 다시 실행 중인 경우 0이 아닌 값을 반환합니다.
![[Pasted image 20241013164117.png|600]]
# 좀비프로세스와 조기 종료

## 좀비 프로세스 (defunct 프로세스)
- 부모가 종료된 자식에 대해 wait()를 호출하지 않으면 좀비 상태가 됩니다 (ps에서 "Z" 상태로 표시됨).
- 부모가 wait() 또는 waitpid()를 호출할 때까지 좀비는 남아 있습니다.
- 이는 실행을 완료했지만 프로세스 테이블에 항목이 남아 있는 프로세스입니다.
![[Pasted image 20241013164610.png|500]]
![[Pasted image 20241013164619.png|500]]
## 고아 프로세스
- 부모 프로세스가 죽으면 자식 프로세스는 고아 프로세스가 됩니다.
- init 프로세스 (pid = 1)가 고아 프로세스의 부모가 됩니다.
- init 프로세스는 주기적으로 자식에 대해 wait를 실행하므로 결국 고아 좀비는 제거됩니다.
![[Pasted image 20241013164701.png|500]]
![[Pasted image 20241013164721.png|400]]
### 5.9 smallsh: 명령어 프로세서

#### 명령어 프로세서의 기본 로직
```c
while (EOF not typed) // EOF가 입력되지 않는 동안
{
    사용자로부터 명령어를 입력받음
    // fgets() 함수를 사용하여 입력을 받음
    명령어 인수를 조립하고 실행
    // excute_cmdline() 함수를 사용하여 실행
}
```
## ls를 실행하는 step
![[Pasted image 20241014143419.png|500]]
#### Fork Code:
• 자식 프로세스의 프로세스 테이블 항목을 할당합니다.
• 부모 프로세스로부터 자식 프로세스의 항목을 채웁니다.
• 자식 프로세스의 스택과 사용자 영역을 할당합니다.
• 부모로부터 자식의 사용자 영역을 채웁니다.
• 자식 프로세스의 PID를 할당합니다.
• 자식 프로세스가 부모의 텍스트를 공유하도록 설정합니다.
• 데이터와 스택의 페이지 테이블을 복사합니다.
• 열린 파일의 공유 설정을 합니다.
• 부모의 레지스터를 자식에게 복사합니다.
#### Exec code
• 실행할 프로그램을 찾습니다.
• 실행 권한을 확인합니다.
• 헤더를 읽고 확인합니다.
• 인수와 환경을 커널로 복사합니다.
• 이전 주소 공간을 해제합니다.
• 새로운 주소 공간을 할당합니다.
• 인수와 환경을 스택으로 복사합니다.
• 신호를 재설정합니다.
• 레지스터를 초기화합니다.
![[Pasted image 20241014143727.png|500]]
## smallsh - shell example
![[Pasted image 20241014145344.png|550]]

입력을 inputbuf에 넣고, 입력 글자 수가 유효한지 확인하는 함수
![[Pasted image 20241014145408.png|550]]
명령을 tok과 tokbuf로 나눠서 읽는다.
![[Pasted image 20241014145420.png|550]]
나눈 명령을 실행한다.
![[Pasted image 20241014145437.png|550]]
![[Pasted image 20241014145446.png|550]]

## 프로세스 속성

## 프로세스 ID 0 & 1
![[Pasted image 20241014120812.png|500]]
- **스케줄러 프로세스 (swapper)**: 프로세스 ID 0
  - 이 프로세스에 해당하는 디스크의 프로그램이 없습니다.
  - 커널의 일부이며 시스템 프로세스로 알려져 있습니다.
- **init 프로세스**: 프로세스 ID 1
  - 부트스트랩 절차가 끝날 때 커널에 의해 호출됩니다.
  - 이 프로세스의 프로그램 파일은 구 버전 UNIX 시스템에서는 /etc/init였고, 최신 버전에서는 /sbin/init입니다.
  - 이 프로세스는 UNIX 시스템을 시작하는 역할을 합니다.
  - init 프로세스는 죽지 않습니다.
  - 이는 커널 내의 시스템 프로세스가 아닌 일반 사용자 프로세스이며, superuser 권한으로 실행됩니다.
![[Pasted image 20241014120453.png|600]]
![[Pasted image 20241014120509.png|600]]
## 프로세스 그룹과 프로세스 그룹 ID (1/2)
- 프로세스 그룹은 하나 이상의 프로세스로 구성되며, 일반적으로 동일한 작업과 관련이 있습니다.
  - 프로세스는 파이프로 연결됩니다.
- 각 프로세스 그룹은 고유한 프로세스 그룹 ID를 가집니다.
- 프로세스 그룹 ID는 프로세스 ID와 유사합니다.
  - 양의 정수입니다.
  - pid_t 데이터 유형입니다.
- 각 프로세스 그룹에는 프로세스 그룹 리더가 있을 수 있습니다.
  - (pid == pgid)인 경우 프로세스 그룹 리더입니다.
```c
#include <unistd.h>
pid_t getpgrp(void);
pid_t getpgid(pid_t pid); Returns: process group ID of calling process
/* getpgid(0) == getpgrp() */
// Returns: process group ID if OK, -1 on error
```
![[Pasted image 20241014121035.png]]
`$ cmd1 | cmd2`는 cmd1과 cmd2가 같은 프로세스 그룹이 되고, shell은 다른 프로세스 그룹이다.
bash가 cmd1과 cmd2를 만들고 그룹을 분리시킨 것
## 프로세스 그룹 변경 (1/2)
```c
#include <unistd.h>
int setpgid(pid_t pid, pid_t pgid);
// Returns: 0 if OK, -1 on error
```
- 이 함수는 프로세스의 프로세스 ID가 pid와 같은 프로세스의 프로세스 그룹 ID를 pgid로 설정합니다.
	-  pid == pgid: 지정된 프로세스 ID(pid)가 프로세스 그룹 리더가 됩니다.
	- pid == 0: 호출자의 프로세스 ID가 사용됩니다.
	- pgid == 0: 지정된 프로세스 ID(pid)가 프로세스 그룹 ID로 사용됩니다.
- **프로세스는 자신이나 자식의 프로세스 그룹 ID만 설정할 수 있다**
- 또한 자식이 exec 함수 중 하나를 호출한 후에는 자식의 프로세스 그룹 ID를 변경할 수 없습니다.
## 세션 및 세션 ID (1/3)
- 세션은 하나 이상의 프로세스 그룹의 모음입니다.
- 세션 내의 프로세스 그룹은 하나의 포어그라운드 프로세스 그룹과 하나 이상의 백그라운드 프로세스 그룹으로 나눌 수 있습니다.
![[Pasted image 20241014122130.png|500]]
모든 프로세스는 controlling terminal이 관리한다.

```c
#include <unistd.h>
pid_t getsid(pid_t pid);
// Returns: session leader's process group ID if OK, -1 on error
```
- pid가 0이면, getsid는 호출 프로세스의 세션 리더의 프로세스 그룹 ID를 반환합니다.
- 데몬 프로세스:
  - 제어 터미널이 없는 단순한 프로세스입니다.
  - 예로는 cron이 있습니다.
```c
#include <unistd.h>
pid_t setsid(void);
// Returns: process group ID if OK, -1 on error
```
- 호출 프로세스가 프로세스 그룹 리더가 아니면, 이 함수는 새로운 세션을 만듭니다.
- 세 가지 일이 발생합니다:
  - 프로세스는 이 새로운 세션의 **세션 리더**가 됩니다. (세션 리더는 세션을 만드는 프로세스입니다.)
  - 프로세스는 새로운 프로세스 그룹의 **프로세스 그룹 리더**가 됩니다.
  - 프로세스는 **제어 터미널이 없다**. 프로세스가 setsid를 호출하기 전에 제어 터미널이 있었다면, 그 연결이 끊어진다.
## 현재 작업 디렉토리 및 루트 디렉토리 
### 현재 작업 디렉토리:
  - 현재 작업 디렉토리는 프로세스를 시작한 fork 또는 exec을 통해 상속됩니다.
  - 프로세스별 속성입니다.
  - 자식 프로세스가 `chdir`을 호출하여 위치를 변경하면, 부모 프로세스의 현재 작업 디렉토리는 변경되지 않습니다.
### 현재 루트 디렉토리:
  - 각 프로세스는 절대 경로 이름 검색에 사용되는 루트 디렉토리와도 연결됩니다.
  - 파일 시스템 계층 구조의 시작 지점입니다.
```c
#include <unistd.h>
int chroot(const char *path);
// Return: 0 if OK, -1 on error
```
  - chroot가 성공하면, path는 '/'로 시작하는 파일 검색의 시작 지점이 됩니다.
  - 호출 프로세스에만 영향을 미치며, 시스템 전체에는 영향을 미치지 않습니다.
![[Pasted image 20241014124502.png|500]]
### 사용자 ID 및 그룹 ID 변경 (1/2)
```c
#include <unistd.h>
int setuid(uid_t uid);
int setgid(gid_t gid);
// Both return: 0 if OK, -1 on error
```
- UNIX 시스템에서, 현재 날짜를 변경하는 등의 특권과 특정 파일을 읽거나 쓸 수 있는 접근 제어는 사용자 및 그룹 ID에 기반합니다.
- setuid 함수를 사용하여 실제 사용자 ID와 유효 사용자 ID를 설정할 수 있습니다.
- 마찬가지로, setgid 함수를 사용하여 실제 그룹 ID와 유효 그룹 ID를 설정할 수 있습니다.

### 파일 크기 제한: ulimit
```c
#include <ulimit.h>
long ulimit(int cmd, [long newlimit]);
// Return: the value of the requested limit if OK, -1 on error
```
- write 시스템 호출로 생성할 수 있는 파일의 크기에는 프로세스별로 제한이 있습니다.
- 현재 파일 크기 제한을 얻으려면:
  - 매개변수 cmd를 UL_GETFSIZE로 설정합니다.
- 파일 크기 제한을 변경하려면:
  - 매개변수 cmd를 UL_SETFSIZE로 설정하고 새로운 제한을 적용합니다.
- 이 방법으로 파일 크기 제한을 실제로 증가시킬 수 있는 것은 슈퍼유저만 가능합니다.
- 다른 사용자의 유효 사용자 ID를 가진 프로세스는 제한을 줄이는 것이 허용됩니다.
## 프로세스 우선순위: nice
- 시스템은 특정 프로세스에 할당되는 CPU 시간의 비율을 부분적으로 정수 nice 값에 기반하여 결정합니다.
- **nice 값 범위**:
  - 0 ~ 시스템에서 우선순위가 가장 높은 값
  - 숫자가 높을수록 프로세스의 우선순위가 낮아집니다.
- 슈퍼유저 프로세스는 nice 호출의 매개변수로 음수 값을 사용하여 우선순위를 높일 수 있습니다.