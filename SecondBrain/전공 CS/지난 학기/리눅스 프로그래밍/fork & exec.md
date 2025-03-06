# 프로세스 환경
• 모든 UNIX 프로세스는 다른 프로세스를 시작할 수 있습니다. (fork, exec...)
• 이는 파일 시스템의 디렉토리 트리와 비슷한 계층 구조로 UNIX 프로세스 환경을 형성합니다.
• 프로세스 트리의 **최상위에는 init**이라는 매우 중요한 프로그램의 실행인 단일 제어 프로세스가 있으며, 이는 모든 시스템 및 사용자 프로세스의 궁극적인 조상입니다.
# fork
```c
#include <unistd.h>
pid_t fork(void);
// Returns: 0 in child, process ID of child in parent, -1 on error
```
호출된 프로세스의 추출된 복제본인 새로운 프로세스를 생성한다.
- 자식 프로세스는 부모 프로세스의 데이터 공간, 힙, 스택의 복사본을 받고 공유하지는 않음, 
- 텍스트 세그먼트는 공유
- 어떤 프로세스가 먼저 실행을 시작할지는 커널에서 사용하는 스케줄링 알고리즘에 따라 달라지므로 알 수 없다.
## fork, 파일 및 데이터
•	부모에서 열려 있는 모든 파일은 자식에서도 열려 있습니다.
•	**부모와 자식이 동일한 파일 오프셋을 공유한다**
•	**자식이 표준 출력에 쓸 때 부모의 오프셋이 업데이트된다**
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
# exec
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
## exec와 열린 파일들
•	**exec도 완전히 새로운 프로그램이 시작될 때 원래 프로그램에서 열려 있던 파일은 그대로 열려 있다**
•	**file offset은 exec 호출에 의해 변경되지 않는다**
•	close-on-exec 플래그(child가 exec을 하면 parent가 open한 file descripter가 자동으로 close됨)가 켜져 있으면(기본값은 꺼져 있음)

다른 프로그램에서 read가 가능하므로 close on exit 플래그를 on하면 좋다.
## exec시 호출 프로세스에서 상속된 속성들
• 실제 사용자 ID 및 실제 그룹 ID 
• 보조 그룹 ID 
• 프로세스 그룹 ID 
• 세션 ID 
• 제어 터미널 
• 알람 시계까지 남은 시간 => 상속
• 현재 작업 디렉토리 
• 루트 디렉토리 
• 파일 모드 생성 마스크 
• 파일 잠금 => 상속
• 프로세스 신호 마스크 
• 보류 중인 신호 => 상속
• 자원 한계 
• tms_utime, tms_stime, tms_cutime, tms_cstime에 대한 값 => 상속