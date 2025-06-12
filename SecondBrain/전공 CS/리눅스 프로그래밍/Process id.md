## 스케쥴러 프로세스
- pid 0
- 이 프로세스에 해당하는 디스크의 프로그램이 없습니다.
- 커널의 일부이며 시스템 프로세스로 알려져 있습니다.
# init 프로세스
- pid 1
- 부트스트랩 절차가 끝날 때 커널에 의해 호출됩니다.
- 이 프로세스의 프로그램 파일은 구 버전 UNIX 시스템에서는 /etc/init였고, 최신 버전에서는 /sbin/init입니다.
- 이 프로세스는 UNIX 시스템을 시작하는 역할을 합니다.
- init 프로세스는 죽지 않습니다.
- 이는 커널 내의 시스템 프로세스가 아닌 일반 사용자 프로세스이며, superuser 권한으로 실행됩니다.
## 프로세스 그룹과 프로세스 그룹 ID
- 프로세스 그룹은 하나 이상의 프로세스로 구성되며, 일반적으로 동일한 작업과 관련이 있습니다.
	- 프로세스는 파이프로 연결됩니다.
- 각 프로세스 그룹은 고유한 프로세스 그룹 ID를 가집니다.
- 각 프로세스 그룹에는 프로세스 그룹 리더가 있을 수 있습니다.
	- (pid == pgid)인 경우 프로세스 그룹 리더입니다.
```c
#include <unistd.h>
pid_t getpgrp(void);
// Returns: process group ID of calling process
pid_t getpgid(pid_t pid);  /* getpgid(0) == getpgrp() */
// Returns: process group ID if OK, -1 on error
```

## 프로세스 그룹 변경
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
## 세션 및 세션 ID
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
## 사용자 ID 및 그룹 ID 변경
```c
#include <unistd.h>
int setuid(uid_t uid);
int setgid(gid_t gid);
// Both return: 0 if OK, -1 on error
```
- UNIX 시스템에서, 현재 날짜를 변경하는 등의 특권과 특정 파일을 읽거나 쓸 수 있는 접근 제어는 사용자 및 그룹 ID에 기반합니다.
- setuid 함수를 사용하여 real user ID와 effective user ID를 설정할 수 있습니다.
- 마찬가지로, setgid 함수를 사용하여 real group ID와 the effective group ID를 설정할 수 있습니다.
## 파일 크기 제한: ulimit
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