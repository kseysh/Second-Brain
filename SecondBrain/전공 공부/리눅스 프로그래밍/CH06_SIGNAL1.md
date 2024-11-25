# Signal Concepts
-  커널에서 일어나는 일
	- 키보드 입력을 담당하는 커널이 인터럽트 문자를 감지한다.
	- SIGINT라는 신호를 모든 프로세스(포어그라운드 그룹)에 보낸다.
	- cc가 이 신호를 받으면, SIGINT와 관련된 기본 동작을 수행하고 종료한다.
- 신호는 소프트웨어 인터럽트이다.
- 신호는 비동기 이벤트를 처리하는 방법을 제공한다.
- 프로세스는 단순히 변수를 테스트하여(예: errno) 신호가 발생했는지 확인할 수 없다.
	-  대신 프로세스는 커널에 “이 신호가 발생하면 다음 작업을 수행하라”고 알려야 한다.

- 모든 신호에는 이름이 있다.
	- 이 이름들은 모두 <signal.h> 헤더에 정의된 양의 정수 상수(신호 번호)로 정의된다.
- 다양한 조건이 신호를 생성할 수 있다.
- 터미널에서 생성된 신호(Control-C -> SIGINT)
- 하드웨어 예외가 신호를 생성한다(0으로 나누기 등).
- kill(2) 함수
- kill(1) 명령어(kill -9 \#pid)
- 소프트웨어 조건(SIGURG, SIGPIPE, SIGALRM 등)

![[Pasted image 20241014130910.png|600]]
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

signal에 대한 default action이 terminate가 아닌 signal
- SIGSTOP (STOP)
- SIGTSTP (STOP)
- SIGCONT(CONTINUE)
- SIGUSR1 (IGNORE)
- SIGUSR2 (IGNORE)

• 신호는 이벤트의 소프트웨어 알림이다.
• 신호는 해당 신호를 발생시키는 이벤트가 발생하면 생성된다.
• 신호는 프로세스가 해당 신호에 기반하여 작업을 취할 때 전달된다.
• 신호의 수명은 생성과 전달 사이의 간격이다.
• 생성되었지만 아직 전달되지 않은 신호는 보류 중이라고 한다.
• 프로세스가 신호 핸들러를 실행할 때 신호를 Catch했다고 한다.
• 프로그램은 sigaction을 호출하여 사용자 정의 함수 이름으로 신호 핸들러를 설치한다.

시그널은 default action을 하거나, ignore 되거나 catch될 수 있다. 하지만, SIGKILL과 SIGSTOP은 default action만 가능하다.
## 정상 비정상 종료
![[Pasted image 20241014131012.png|600]]
# Signal handling
- 신호가 발생하면 세 가지 작업 중 하나를 수행한다.
	- 무시 동작
		-  하지만 SIGKILL과 SIGSTOP 두 신호는 무시할 수 없다.
	- 사용자 정의 동작 (catch)
		- 이를 위해 신호가 발생할 때마다 커널이 우리의 함수를 호출하도록 지시한다(신호 핸들러).
		- SIGKILL과 SIGSTOP 두 신호는 잡을 수 없음을 유의한다.
	- 기본 동작
		- 모든 신호에는 기본 동작이 있다.
		- 기본 동작은 일반적으로 프로세스를 종료시키는 것이다.
## 신호 처리: 사용자 정의 동작
• 잡힌 신호가 프로세스에 의해 처리될 때, 프로세스가 실행 중인 정상적인 명령어 시퀀스는 신호 핸들러에 의해 일시 중단된다.
• 그런 다음 프로세스는 계속 실행되지만 신호 핸들러의 명령어를 실행한다.
![[Pasted image 20241014134019.png|500]]
신호 핸들러가 반환되면, 신호가 잡혔을 때 프로세스가 실행 중이던 정상적인 명령어 시퀀스가 계속 실행된다.
• 그러나 신호 핸들러 내에서는 신호가 잡혔을 때 프로세스가 어디에서 실행 중이었는지 알 수 없다.
## 프로세스 신호 마스크 – 프로세스 속성
• 신호가 생성될 때 취해지는 동작은 현재 신호 핸들러와 프로세스 신호 마스크에 따라 다르다.
• 신호 마스크는 차단될 신호 목록을 포함한다.
• *프로그램은 `sigprocmask`를 사용하여 프로세스 신호 마스크를 변경하여 신호를 차단*한다.
• *프로세스는 fork와 exec 이후에도 signal mask를 상속*한다.

이 방식을 사용하면 SIGKILL 같은 handle할 수 없는 SIGNAL도 block시킬 수 있다.

---- 기말 범위 -----
## signal(2) 시스템 호출(1/2)
```c
#include <signal.h>
void (*signal(int signo, void (*func)(int)))(int);
// Returns: previous disposition of signal (see following) if OK, SIG_ERR on error
```
- signal에 대한 action을 정의하는 함수

• signal 함수는 ISO C에 정의되어 있지만, signal의 의미가 구현마다 다르기 때문에, sigaction 함수를 사용하는 것이 좋다.
![[Pasted image 20241125220146.png|400]]

![[Pasted image 20241028120807.png|600]]
pause() => signal을 받을 때까지 기다리는 것
SIGTERM을 보냈을 때는 sig_usr가 실행되지 않고 default action을 하며 꺼진다 (signal을 setting 해주지 않았기 때문)

![[Pasted image 20241028120824.png|600]]
## Signal Block
- 프로세스 신호 마스크는 차단할 신호의 리스트입니다.
- *sig_int 함수가 시작 될 때 프로세스 신호 마스크가 추가되어 SIGINT를 차단하고, sig_int 함수가 끝나면 프로세스 신호 마스크가 끝나 차단이 해제*된다.
- *신호 큐가 없으므로, UNIX 커널은 신호를 한 번만 전달한다*.
![[Pasted image 20241028121253.png|500]]
## Signal handling & `exec` (1/2)
- 프로그램이 실행될 때 모든 신호의 상태는 기본값이거나 무시 상태입니다.
- 프로세스가 `exec`를 호출하면, 기존의 signal handling이 없어진다.
- 프로세스가 `fork`를 호출하면, 자식 프로세스는 부모의 신호 처리를 상속받습니다.
  - 이 경우, 자식은 부모의 메모리 이미지를 복사받으므로, signal handling이 자식도 유효하다.
![[Pasted image 20241028122115.png|500]]
![[Pasted image 20241028122332.png|500]]
![[Pasted image 20241028122346.png|500]]
ctrl c는 foreground에만 적용되어서, background에는 kill -INT 1828을 해준 것.

ps 이후 결과
1828이 없어짐
1828이 없는데 신호를 보내서 그대로임
## Signal Sets
- 여러 신호를 표현하는 데이터 타입, 즉 **신호 집합**이 필요합니다.
- 예를 들어, `sigprocmask` 같은 함수에서 커널에게 이 신호 집합의 신호가 발생하지 않도록 지시할 수 있습니다.
(예시 안 봄)
## `sigaction(2)` 시스템 호출 (1/3)
```c
#include <signal.h>
int sigaction(int signo, const struct sigaction *restrict act,
struct sigaction *restrict oact);
// Returns: 0 if OK, -1 on error
```
- `sigaction` 함수는 특정 신호에 대해 동작을 조회하거나 수정할 수 있습니다. 이 함수는 UNIX 시스템의 `signal` 함수를 대체합니다.
  - **인자**
    - `signo`: 신호 번호
    - `act`: 동작 수정 시 사용
    - `oact`: 이전 동작
```c
struct sigaction {
void (*sa_handler)(int); /* addr of signal handler, */ /* or SIG_IGN, or SIG_DFL */
	sigset_t sa_mask; /* additional signals to block */		
	int sa_flags; /* signal options, Figure 10.16 */

	void (*sa_sigaction)(int, siginfo_t *, void *);	/* alternate handler */

};
```
  - **주요 필드**
    - `sa_handler`: 신호를 잡는 함수의 주소 (또는 상수 `SIG_IGN`이나 `SIG_DFL`)
    - `sa_mask`: 신호 잡기 함수가 호출되기 전 프로세스의 신호 마스크에 추가되는 신호 집합
    - `sa_flags`: 신호 처리 옵션 (`SA_INTERRUPT`, `SA_NOCLDSTOP`, 등)
    - `sa_sigaction`: `SA_SIGINFO` 플래그를 사용할 때 대체 신호 처리기에 사용됩니다.
      - 일부 구현은 `sa_handler`와 `sa_sigaction`에 동일한 저장 공간을 사용하므로 둘 중 하나만 사용할 수 있습니다.
![[Pasted image 20241028123802.png|500]]
static을 사용한 이유 -> 메모리를 clear하기 위해서(전역변수는 값을 clear해주기 때문)
![[Pasted image 20241028123959.png|500]]
![[Pasted image 20241028124013.png|500]]
![[Pasted image 20241028124053.png|500]]
## 신호와 시스템 호출 (1/3)
- 프로세스가 시스템 호출 중 신호를 받으면, 시스템 호출이 완료될 때까지 신호는 영향을 미치지 않습니다.
- 만약 프로세스가 "느린" 시스템 호출 동안 신호를 잡으면, 시스템 호출이 중단됩니다.
  - 시스템 호출이 오류를 반환하고, 오류 번호(`errno`)가 `EINTR`로 설정됩니다.
- 시스템 호출은 두 가지로 나뉩니다:
  - **"느린" 시스템 호출**: 무기한 대기할 수 있는 호출 (파이프, 터미널, 네트워크 장치 등)
  - **나머지 호출**: 디스크 I/O 관련 호출은 예외로 느리지 않습니다.
- 중단된 시스템 호출을 처리하는 오류가 복잡해질 수 있어, 4.2BSD에서는 중단된 시스템 호출을 자동으로 재시작하는 기능을 도입했습니다.
  - **자동 재시작 시스템 호출**: `ioctl`, `read`, `write`, `wait`, `waitpid` 등이 있으며, 신호가 잡히면 재시작됩니다.
  - `sigaction`의 `sa_flags` 변수에 `SA_RESTART` 플래그를 설정하면, 시스템 호출이 자동으로 재시작됩니다.

### `sigaction(2)`의 또 다른 신호 처리기
- `siginfo_t` 구조체는 신호가 생성된 이유에 대한 정보를 포함합니다.
- 신호 잡기 함수가 반환되면, 프로세스의 신호 마스크는 이전 값으로 재설정됩니다.
- 특정 신호를 잡는 동안 추가적인 같은 신호가 발생하면 해당 신호는 차단됩니다.

### `sigsetjmp(3)`와 `siglongjmp(3)` (1/2)
- 이 두 함수는 신호 처리기에서 분기할 때 사용해야 합니다.
  - `savemask`가 0이 아니면, `sigsetjmp`는 프로세스의 현재 신호 마스크를 저장합니다.
  - `siglongjmp`가 호출되면, `sigsetjmp`로 저장된 신호 마스크가 복원됩니다.