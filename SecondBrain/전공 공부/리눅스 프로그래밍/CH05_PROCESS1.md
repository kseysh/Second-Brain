# mainFunction
• 메인 함수의 프로토타입
```c
int main(int argc, char *argv[]);
```
• 인수
	• argc는 명령줄 인수의 수입니다.
	• argv는 인수에 대한 포인터 배열입니다.
• C 프로그램이 exec 함수 중 하나에 의해 커널에 의해 실행될 때, 메인 함수가 호출되기 전에 특별한 시작 루틴이 호출됩니다.
![[Pasted image 20241011235503.png|200]]
• 실행 가능한 프로그램 파일은 이 루틴을 프로그램의 시작 주소로 지정합니다.
• 이 시작 루틴은 커널로부터 값을 받아옵니다:
	• 명령줄 인수(argc, argv 세팅)
	• Environment - 실행환경 세팅
## Command-Line Argument
• 프로그램이 실행될 때, exec을 수행하는 프로세스는 새로운 프로그램에 명령줄 인수를 전달할 수 있습니다.
![[Pasted image 20241007124549.png|450]]
![[Pasted image 20241007124140.png|200]]
### 환경 변수 리스트
• 환경 리스트는 문자 포인터의 배열입니다.
• 포인터 배열의 주소는 전역 변수 `environ`에 포함되어 있습니다:

• 특정 환경 변수를 접근하기 위해서는 보통 `environ` 변수를 통해서가 아닌 `getenv`와 `putenv` 함수를 통해서 합니다.
![[Pasted image 20241007125006.png|300]]
• 대부분의 UNIX 시스템은 메인 함수에 세 번째 인수를 제공합니다.
사실 main함수는 envp라는 세번째 인수를 가지고 있다.
![[Pasted image 20241007124947.png]]
# 메모리 레이아웃 C 프로그램
• C 프로그램은 다음으로 구성됩니다:
	• 텍스트(코드) 세그먼트:
		• 실행할 프로그램 명령어(기계 명령어)
	• 초기화된 데이터 세그먼트:
		• 초기화된 global 및 static 변수
		`int maxcount = 99;`
	• 초기화되지 않은 데이터 세그먼트(bss):
		• 초기화되지 않은 global 및 static 변수
		• 이 세그먼트의 데이터는 프로그램이 실행되기 전에 커널에 의해 0 또는 null 포인터로 초기화됩니다.
		`long sum[1000];`
• 스택:
	• 함수에 로컬인 변수를 저장합니다.
	• 함수가 호출되면, 그 함수의 자동 변수들이 스택의 맨 위에 할당됩니다.
	• 함수가 종료되면 그 변수들은 할당 해제됩니다.
• 힙:
	• 동적 메모리는 힙에서 할당됩니다.
• **초기화되지 않은 데이터 세그먼트의 내용은 디스크에 있는 프로그램 파일에 저장되지 않습니다**.
• 프로그램 파일에 저장되어야 하는 프로그램의 유일한 부분은 `텍스트 세그먼트`와 `초기화된 데이터`입니다.
![[Pasted image 20241012001001.png|400]]

![[Pasted image 20241007125455.png|300]]
초기화 해서 첫 번째 것은 data가 200272지만, 두 번째 것은 data가 252밖에 안됨
# 프로세스(1/2)
• 실행 중인 프로그램의 인스턴스입니다.
• 프로세스는 프로그램의 실행에 해당합니다.(프로세스는 매순간 내용이 달라짐)
• 프로그램 ≠ 프로세스

• 프로세스는 다음을 포함합니다:
	• 프로그램 코드
	• 프로그램 변수 내의 데이터 값
	• 하드웨어 레지스터
	• 프로그램 스택 (stack, heap)
• 프로세스는 프로세스 ID(pid)에 의해 식별됩니다.
• 셸은 새로운 프로세스를 생성합니다.
![[Pasted image 20241012003525.png]]
유저가 cat을 실행하여 cat process를 생성하는 예제
pipe를 이용해서 ls에서의 출력 값을 wc에 입력하는 예제
• 프로세스 환경
	• 모든 UNIX 프로세스는 다른 프로세스를 시작할 수 있습니다. (fork, exec...)
	• 이는 파일 시스템의 디렉토리 트리와 비슷한 계층 구조로 UNIX 프로세스 환경을 형성합니다.
	• 프로세스 트리의 **최상위에는 init**이라는 매우 중요한 프로그램의 실행인 단일 제어 프로세스가 있으며, 이는 모든 시스템 및 사용자 프로세스의 궁극적인 조상입니다.
• UNIX는 프로세스 생성 및 조작을 위한 몇 가지 시스템 호출을 제공합니다:
	• fork, exec, wait, exit 등
# getpid(2)와 getppid(2) 시스템 호출
```c
#include <unistd.h>
pid_t getpid(void);
// Returns: the process ID of the calling process
pid_t getppid(void);
// Returns: the parent process ID of the calling process
```
프로세스 ID
	• 모든 프로세스는 고유한 프로세스 ID, 즉 0 이상의 정수를 가집니다.
	• 고유하지만, 프로세스 ID는 재사용됩니다.
## fork(2) system call
```c
#include <unistd.h>
pid_t fork(void);
// Returns: 0 in child, process ID of child in parent, -1 on error
```
호출된 프로세스의 추출된 복제본인 새로운 프로세스를 생성한다.
- 자식 프로세스는 부모 프로세스의 데이터 공간, 힙, 스택의 복사본을 받지만 공유하지는 않음
- 하지만, 텍스트 세그먼트는 공유합니다.
- 어떤 프로세스가 먼저 실행을 시작할지는 커널에서 사용하는 스케줄링 알고리즘에 따라 달라지므로 알 수 없다.
 ![[Pasted image 20241012005342.png|600]]
텍스트 세그먼트를 공유하는 것을 볼 수 있다.

![[Pasted image 20241007130721.png|600]]
자식(B)은 fork()가 실행된 이후 Two를 각각 실행하게 된다.
parent와 child는 똑같은 fork()를 실행하지만, child는 pid를 0으로 반환한다.

**오류 처리 (EAGAIN)**
• 시스템 전체에서 생성할 수 있는 프로세스의 수에 제한이 있을 때 발생하는 오류입니다.
• 개별 사용자가 생성할 수 있는 프로세스 수에 제한이 있을 때도 발생할 수 있습니다.

![[Pasted image 20241007120856.png]]
그냥 child와 parent가 다른 함수를 실행할 수 있다는 예제
## exec family
• exec 패밀리는 새로운 프로그램의 실행을 시작하는 데 사용할 수 있습니다. (자기 자신을 새로운 프로그램을 시작하도록 바꿈)
• 새로운 프로세스가 생성되지 않으므로 프로세스 ID는 변경되지 않습니다.
• exec는 단순히 현재 프로세스의 텍스트, 데이터, 힙, 스택 세그먼트를 디스크에 있는 새로운 프로그램으로 대체합니다.
• execve만이 커널 내의 시스템 호출이다. 나머지 exec family는 library다
```c
#include <unistd.h>
int execl(const char *pathname, const char *arg0,…/*NULL*/ );
int execv(const char *pathname, char *const argv[]);
int execle(const char *pathname, const char *arg0,…/*NULL*/,char *const envp[]);
int execve(const char *pathname, char *const argv[], char *const envp[]);
int execlp(const char *filename, const char *arg0,.../*NULL*/ );
int execvp(const char *filename, char *const argv[]);
// All six return: -1 on error, **no return on success**
```
l / v - e / p 가 뒤에 붙는다.
execve만 system call이다.
l - list 인자들을 리스트처럼 나열하고 마지막은 NULL로 끝남 (`arg0`, `arg1` ...)
v - vector - 인수들이 배열 형태(`argv[]`)로 전달됨
e - environment - 환경변수 배열(`envp[]`)을 함께 전달함
p - path를 뜻하고, 파일의 경로를 직접 명시하지 않고 시스템의 PATH 환경변수를 사용하여 실행파일을 찾는다. - 따라서 filename만 받음
```jsx
$ echo $PATH
PATH=/bin:/usr/bin:/usr/local/bin/:.
```
##### ex)
![[Pasted image 20241012214101.png|550]]
![[Pasted image 20241012214157.png|550]]
- Accessing arguments passed with `exec`
![[Pasted image 20241012214210.png|550]]
## `exec`와 `fork` 사용
![[Pasted image 20241012214416.png|550]]
![[Pasted image 20241012214432.png|550]]
## `decommand` example
![[Pasted image 20241012214514.png|550]]
다음 문자열 인자를 표준 입력 대신 명령으로 처리하라는 의미에서 -c 인자를 쉘 호출 시 사용합니다.
## fork, 파일 및 데이터
•	부모에서 열려 있는 모든 파일은 자식에서도 열려 있습니다.
•	**부모와 자식이 동일한 파일 오프셋을 공유한다**
•	**자식이 표준 출력에 쓸 때 부모의 오프셋이 업데이트된다**
![[Pasted image 20241012215234.png|550]]
printpos -> 현재 파일 디스크립터의 offset을 프린트하는 함수 (print position)

Before fork: 10
Child before read: 10
Child after read: 20
Parent after wait: 20
## 부모 프로세스에서 상속된 속성들
•	실제 사용자 ID, 실제 그룹 ID, 유효 사용자 ID, 유효 그룹 ID
•	보조 그룹 ID
•	프로세스 그룹 ID
•	세션 ID
•	제어 터미널
•	set-user-ID 및 set-group-ID 플래그
•	현재 작업 디렉토리
•	루트 디렉토리
•	파일 모드 생성 마스크
•	신호 마스크 및 처리 방식
•	모든 열린 파일 디스크립터에 대한 close-on-exec 플래그
•	환경
•	연결된 공유 메모리 세그먼트
•	메모리 매핑
•	자원 한계
=> 대부분이 상속된다.
## 부모와 자식의 차이점
•	**fork의 반환 값**
•	프로세스 ID
•	두 프로세스는 다른 부모 프로세스 ID를 가진다.
•	자식의 tms_utime, tms_stime, tms_cutime, tms_cstime 값은 0으로 설정됩니다. (process time은 다름)
•	부모에 의해 설정된 파일 잠금은 자식에게 상속되지 않습니다.
•	자식의 보류 중인 알람은 지워집니다.
•	자식의 보류 중인 신호 세트는 빈 세트로 설정됩니다.
## exec와 열린 파일들
•	**exec도 완전히 새로운 프로그램이 시작될 때 원래 프로그램에서 열려 있던 파일은 그대로 열려 있다**
•	**file offset은 exec 호출에 의해 변경되지 않는다**
•	close-on-exec 플래그(child가 exec을 하면 parent가 open한 file descripter가 자동으로 close됨)가 켜져 있으면(기본값은 꺼져 있음)
![[Pasted image 20241012220334.png|600]]
![[Pasted image 20241012220430.png|600]]
다른 프로그램에서 read가 가능하므로 close on exit 플래그를 on하면 좋다.
## 호출 프로세스에서 상속된 속성들
• 실제 사용자 ID 및 실제 그룹 ID • 보조 그룹 ID • 프로세스 그룹 ID • 세션 ID • 제어 터미널 • 알람 시계까지 남은 시간 • 현재 작업 디렉토리 • 루트 디렉토리 • 파일 모드 생성 마스크 • 파일 잠금 • 프로세스 신호 마스크 • 보류 중인 신호 • 자원 한계 • tms_utime, tms_stime, tms_cutime, tms_cstime에 대한 값