# Signal Concepts
-  커널에서 일어나는 일
	- 키보드 입력을 담당하는 커널이 인터럽트 문자를 감지한다.
	- SIGINT라는 신호를 모든 프로세스(포어그라운드 그룹)에 보낸다.
	- 신호를 받으면, SIGINT와 관련된 기본 동작을 수행하고 종료한다.
- 신호는 비동기 이벤트를 처리하는 방법을 제공한다.
• 프로그램은 sigaction을 호출하여 사용자 정의 함수 이름으로 신호 핸들러를 설치한다.
시그널은 default action을 하거나, ignore 되거나 catch될 수 있다. 하지만, SIGKILL과 SIGSTOP은 default action만 가능하다.
## 시그널 종류
• SIGABRT : abort 함수 호출에 의해 생성됨.
• SIGALRM : 설정된 타이머가 만료되면 생성됨.
• SIGCHLD : 프로세스가 종료되거나 멈출 때마다 부모 프로세스에 신호가 보내짐.
• SIGCONT : 멈춘 프로세스가 계속될 때 보내지는 신호.
• SIGFPE : 0으로 나누기, 부동 소수점 오버플로 등 산술 예외를 알림.
• SIGILL : 프로세스가 불법 하드웨어 명령어를 실행했음을 알림.
• SIGINT : 인터럽트 키를 입력할 때 터미널 드라이버에 의해 생성되어 포어그라운드 프로세스 그룹의 모든 프로세스에 보내짐.
• SIGKILL : **잡을 수 없고 무시할 수 없다.** 어떤 프로세스든 확실히 종료시킬 수 있는 방법.
• SIGPIPE : 파이프라인에 쓰기 시도 시 리더가 종료된 경우 생성됨.
• SIGSEGV : 프로세스가 유효하지 않은 메모리 참조를 했음을 알림. (코어 덤프)
• SIGTERM : 기본적으로 kill(1) 명령어에 의해 보내지는 종료 신호.
• SIGTSTP : 터미널 드라이버에서 Cntl-Z가 입력되어 포어그라운드 프로세스 그룹의 모든 프로세스에 보내짐.
• SIGUSR1 : 사용자 정의 신호 1
• SIGUSR2 : 사용자 정의 신호 2
• SIGSTOP : 잡 제어 신호로, **잡을 수 없고 무시할 수 없다.**
#### signal에 대한 default action이 terminate가 아닌 signal
- SIGSTOP (STOP)
- SIGTSTP (STOP)
- SIGCONT(CONTINUE)
- SIGUSR1 (IGNORE)
- SIGUSR2 (IGNORE)
# Signal handling
- 신호가 발생하면 세 가지 작업 중 하나를 수행한다.
	- 무시 동작
	- 사용자 정의 동작 (catch)
	- 기본 동작
		- 기본 동작은 일반적으로 프로세스를 종료시키는 것이다.
- - SIGKILL과 SIGSTOP 두 신호는 ignore하거나 catch할 수 없다.
## 신호 처리: 사용자 정의 동작
• 잡힌 신호가 프로세스에 의해 처리될 때, 프로세스가 실행 중인 정상적인 명령어 시퀀스는 신호 핸들러에 의해 일시 중단된다.
• 그런 다음 프로세스는 계속 실행되지만 신호 핸들러의 명령어를 실행한다.
신호 핸들러가 반환되면, 신호가 잡혔을 때 프로세스가 실행 중이던 정상적인 명령어 시퀀스가 계속 실행된다.
• 그러나 신호 핸들러 내에서는 신호가 잡혔을 때 프로세스가 어디에서 실행 중이었는지 알 수 없다.

## signal(2) 시스템 호출(1/2)
```c
void (*signal(int signo, void (*func)(int)))(int);
// Returns: previous disposition of signal (see following) if OK, SIG_ERR on error
```
- signal에 대한 action을 정의하는 함수
- signal의 의미가 구현마다 다르기 때문에, sigaction 함수를 사용하는 것이 좋다.
## Signal Block
- *sig_int 함수가 시작 될 때 프로세스 신호 마스크가 추가되어 자동적으로 SIGINT를 차단하고, sig_int 함수가 끝나면 프로세스 신호 마스크가 끝나 차단이 해제*된다.
- *signal queue가 없으므로, UNIX 커널은 신호를 한 번만 전달한다*. (한 개의 SIGNAL만 pending되어 기다린다.)
- sleep 도중에 SIGINT를 처리하게 되면 recursive하게 signal function이 들어갈 수 있다. 따라서 process signal mask를 이용해 SIGNAL을 block한다.
- SIGINT가 block되면, 사라지는 것이 아니라 pending 되는 것이다. 따라서 sigprocmask가 해제되면 다시 sig_int를 실행한다.
![[Pasted image 20241028121253.png|500]]
## Signal handling with `exec & fork`
- *프로세스가 `exec`를 호출하면, 기존의 signal handling이 없어진다*. 
	- (exec는 부모와 실행하는 프로그램이 달라 부모의 signal handling function을 찾지 못하기 때문)
- *프로세스가 `fork`를 호출하면, 자식 프로세스는 부모의 신호 처리를 상속*받습니다. 
	- (fork는 부모와 실행하는 프로그램이 같아 부모의 signal handling function을 찾을 수 있기 때문)
## Signal Sets
```c
#include <signal.h>

int sigemptyset(sigset_t *set); // 전부 0으로 채움
int sigfillset(sigset_t *set); // 전부 1로 채움
int sigaddset(sigset_t *set, int signo); // signo만 1로 set
int sigdelset(sigset_t *set, int signo); // signo만 0으로 set
int sigismember(const sigset_t *set, int signo);
// signo에 해당하는 signal이 1로 세팅되어 있는지를 묻는 것
```
- 여러 신호를 표현하는 신호 집합
- `sigprocmask` 같은 함수에서 커널에게 이 신호 집합의 신호가 발생하지 않도록 지시할 수 있습니다.
## `sigaction(2)` 시스템 호출
```c
int sigaction(int signo, const struct sigaction *restrict act, struct sigaction *restrict oact);
// Returns: 0 if OK, -1 on error
```
- `sigaction` 함수는 특정 신호에 대해 동작을 조회하거나 수정할 수 있습니다.
  - **인자**
    - `signo`: 신호 번호
    - `act`: 동작 수정 시 사용
    - `oact`: 이전 동작을 저장해둘 때 사용
```c
struct sigaction {
	void (*sa_handler)(int); /* addr of signal handler, */ /* or SIG_IGN, or SIG_DFL */
	sigset_t sa_mask; /* additional signals to block */		
	int sa_flags; /* signal options, Figure 10.16 */
	void (*sa_sigaction)(int, siginfo_t *, void *);	/* alternate handler */
};
```
  - **주요 필드**
    - `sa_handler`: signal handler 주소 (또는 상수 `SIG_IGN`이나 `SIG_DFL`)
    - `sa_mask`: 신호 잡기 함수가 호출되기 전 프로세스의 신호 마스크에 추가되는 신호 집합 (이미 block되는 signal외에 추가적으로 block하고 싶은 signal이 있을 때)
    - `sa_flags`: 신호 처리 옵션 (`SA_RESTART`, `SA_SIGINFO`)
sigemptyset을 사용하여 구조체의 sa_mask 멤버를 초기화해야 합니다. act.sa_mask = 0;이 같은 역할을 한다고 보장할 수 없습니다.

• *sa_sigaction 필드는 SA_SIGINFO 플래그가 sigaction과 함께 사용될 때 사용하는 대체 신호 핸들러*입니다.
• 구현체들은 sa_sigaction 필드와 sa_handler 필드를 동일한 저장소를 사용할 수 있으므로, 애플리케이션은 이 두 필드 중 하나만 사용할 수 있다.
• *일반적으로, 신호 핸들러는 다음과 같이 호출*됩니다:
`void handler(int signo);`
• 하지만 *SA_SIGINFO 플래그가 설정된 경우, 신호 핸들러는 다음과 같이 호출*됩니다:
`void handler(int signo, siginfo_t *info, void *context);`

## slow system call
- *프로세스가 시스템 호출 중 신호를 받으면, 시스템 호출이 완료될 때까지 신호는 영향을 미치지 않습니다*.
- *만약 프로세스가 slow system call 동안 신호를 잡으면, 시스템 호출이 중단됩니다*.
  - 시스템 호출이 오류를 반환한다. (system call은 kernel에서 실행되고, user process가 중단을 시키므로 kernel과 user process는 다른 process이므로 kernel에서 중단된 위치로 다시 돌아갈 수 없기 때문)
  - slow system call: 무기한 대기할 수 있는 호출
	  - 파이프, 터미널 장치 I/O, 네트워크 장치 I/O
	  - pause(), wait()
	  - Certain `ioctl` operations
	  - ipc function중 일부
  - 디스크 I/O 관련 호출은 예외로 slow system call에 포함되지 않는다.

- 중단된 시스템 호출을 처리하는 오류가 복잡해질 수 있어, 4.2BSD에서는 중단된 시스템 호출을 자동으로 재시작하는 기능을 도입했습니다.
  - *자동 재시작 시스템 호출: `ioctl`, `read`, `write`, `wait`, `waitpid` 등이 있으며, 신호가 잡히면 재시작*됩니다.
  - *`sigaction`의 `sa_flags` 변수에 `SA_RESTART` 플래그를 설정하면, errno가 설정되지 않고 시스템 호출이 자동으로 재시작*됩니다.
	  - setting해준 signal만 재시작이 된다.
## `sigaction(2)`의 또 다른 신호 처리기
- `siginfo_t` 구조체는 신호가 생성된 이유에 대한 정보를 포함합니다.
	- si_signo -> signal number
	- si_pid -> signal을 보낸 pid
	- si_uid -> signal을 보낸 real user id
## signal에 관한 추가 정보
- signal catching function이 반환되면, 프로세스의 signal mask는 이전 값으로 재설정됩니다.
- 특정 신호를 잡는 동안 추가적인 같은 신호가 발생하면 해당 신호는 차단됩니다.
• OS는 핸들러가 호출될 때 전달되는 신호를 신호 마스크에 포함시킵니다.
• 따라서, 주어진 신호를 처리하는 동안 해당 신호의 또 다른 발생은 우리가 첫 번째 발생을 처리할 때까지 차단됩니다.
• 같은 신호의 추가 발생은 일반적으로 큐에 쌓이지 않습니다.

## `sigprocmask(2)`
```c
int sigprocmask(int how, const sigset_t *restrict set, sigset_t *restrict oset);
```
- 프로세스의 신호 마스크는 해당 프로세스에 전달이 차단될 신호들의 집합입니다.
- `set`이 널 포인터인 경우, 프로세스의 신호 마스크는 변경되지 않으며, `how` 인자는 무시됩니다.
###### `how`
- **SIG_BLOCK**: 차단할 신호 집합에 특정 신호를 추가합니다.
- **SIG_UNBLOCK**: 차단할 신호 집합에서 특정 신호를 제거합니다.
- **SIG_SETMASK**: 지정된 신호 집합으로 차단할 신호를 설정합니다. (기존 것에 추가/삭제가 아닌 아예 새로 지정)

# 신호 보내기
## `kill(2)`와 `raise(2)` 시스템 호출 (1/2)
```c
int kill(pid_t pid, int signo);
int raise(int signo);
```
- `kill` 함수는 프로세스나 프로세스 그룹에 신호를 보냅니다.
- `raise` 함수는 프로세스가 자신에게 신호를 보낼 수 있도록 합니다.
###### `pid` 값에 따른 동작
- `pid > 0`: 프로세스 ID가 `pid`인 프로세스에 신호가 전송됩니다. (특정 pid 전송)
- `pid == 0`: 신호가 송신자의 프로세스 그룹 ID와 같은 프로세스 그룹 ID를 가진 모든 프로세스에 전송됩니다. (송신자의 그룹에 다 전송)
- `pid < -1`: 절대값이 `pid`인 프로세스 그룹 ID를 가진 모든 프로세스에 신호가 전송됩니다. (그룹에 전송)
- `pid == -1`: 송신자가 신호를 보낼 권한이 있는 시스템의 모든 프로세스에 신호가 전송됩니다. (다 전송)
###### 신호를 보낼 권한
- 슈퍼 유저는 모든 프로세스에 신호를 보낼 수 있습니다.
- 송신자의 실제 또는 유효 사용자 ID가 수신자의 실제 또는 유효 사용자 ID와 같아야 합니다.
## Null Signal
- 신호 번호 0을 널 신호로 정의합니다.
  - `signo` 인자가 0일 때, 특정 프로세스가 여전히 존재하는지 확인하는 데 자주 사용됩니다.
  - 프로세스가 존재하지 않으면, `kill`은 -1을 반환하고 `errno`는 `ESRCH`로 설정됩니다.
- 프로세스 존재 여부 테스트는 원자적이지 않으므로 `kill`이 호출자에게 응답을 반환할 때쯤 해당 프로세스가 이미 종료되었을 가능성이 있습니다. 따라서 이 정보의 유효성은 제한적입니다.
## `alarm(2)` 시스템 호출 (1/2)
```c
unsigned int alarm(unsigned int seconds);
```
- `alarm()`은 지정된 시간이 지나면 `SIGALRM` 신호가 생성하도록 설정..
- 프로세스당 하나의 알람 클럭만 설정할 수 있습니다.
- `alarm` 호출 시 이전에 등록된 알람 클럭이 만료되지 않은 경우:
	- 해당 알람 클럭에 남은 시간이 반환되고, 이전 알람 클럭이 새로운 값으로 대체됩니다.