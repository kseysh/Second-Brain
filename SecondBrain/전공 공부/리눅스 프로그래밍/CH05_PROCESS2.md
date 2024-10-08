### 5.6 exit 시스템 호출로 프로세스 종료

#### 프로세스 종료
- 프로세스는 여덟 가지 방법으로 종료될 수 있습니다.
- 정상 종료는 다섯 가지 방법으로 발생합니다:
  - main 함수에서 반환
  - exit 호출
  - _exit 또는 _Exit 호출
  - 마지막 스레드가 시작 루틴에서 반환
  - 마지막 스레드에서 pthread_exit 호출

- 비정상 종료는 세 가지 방법으로 발생합니다:
  - abort 호출
  - 신호 수신
  - 마지막 스레드가 취소 요청에 응답

#### exit(3) 시스템 호출
- exit의 인수는 프로세스의 종료 상태를 나타냅니다.
  - 하위 8비트는 부모 프로세스에게 전달됩니다 (wait 시스템 호출).
- 관례적으로, 프로세스는 다음을 반환합니다:
  - 정상 종료 시 0
  - 문제가 발생하면 0이 아닌 값
- _exit와 _Exit는 즉시 커널로 반환됩니다.
- exit는 특정 정리 작업을 수행합니다.
  - 표준 I/O 라이브러리의 깨끗한 종료 (fclose)
- exit(0)은 main()에서 return(0)과 동일합니다.

#### atexit(3)
- ISO C에서는 프로세스가 exit에 의해 자동으로 호출되는 최대 32개의 함수를 등록할 수 있습니다.
- 이러한 함수들은 exit 핸들러라고 불리며 atexit 함수를 호출하여 등록됩니다.
- exit 함수는 이 함수들을 등록된 역순으로 호출합니다.
- 플랫폼에 의해 지원되는 최대 exit 핸들러 수

### 5.7 프로세스 동기화

#### 자식 프로세스와의 동기화
- 프로세스가 종료되면 (정상적이든 비정상적이든)
  - 커널은 프로세스의 모든 열린 디스크립터를 닫고, 사용 중이던 메모리를 해제합니다.
  - 커널은 종료된 프로세스에 대한 proc 엔트리만 유지합니다 (pid, 종료 상태 - exit(status), CPU 시간).
  - 이 정보는 종료된 프로세스의 부모 프로세스가 확인할 때까지 남아 있습니다.
  - 부모 프로세스는 wait() 시스템 호출을 통해 이 정보를 확인합니다.
  - wait() 후에 종료된 프로세스의 proc 엔트리는 proc 테이블에서 해제됩니다.

#### wait(2) 시스템 호출 (1/2)
- wait는 자식 프로세스가 실행되는 동안 프로세스의 실행을 일시 중지합니다.
- 자식이 종료되면 대기 중인 부모 프로세스가 다시 시작됩니다.
- 여러 자식 프로세스가 실행 중인 경우, wait는 부모의 자식 중 하나가 종료되면 반환됩니다.
- wait가 (pid_t) -1을 반환하면, 자식이 없음을 의미할 수 있습니다.
  - errno = ECHILD
- 부모 프로세스는 자식 중 하나가 종료될 때까지 루프에서 기다릴 수 있습니다.

#### waitpid(2) 시스템 호출 (1/2)
- 언급했듯이, 여러 자식이 있는 경우 wait는 자식 중 하나가 종료될 때 반환됩니다.

#### wait(2) & waitpid(2) 함수의 인수
- 두 함수의 차이점:
  - wait 함수는 호출자를 자식 프로세스가 종료될 때까지 블록할 수 있습니다. 반면에 waitpid는 블록을 방지하는 옵션이 있습니다 (WNOHANG).
  - waitpid 함수는 종료된 자식을 기다리지 않으며, 어떤 프로세스를 기다릴지 제어하는 여러 옵션이 있습니다.

#### 종료 상태 확인 (2/2)

### 5.8 좀비와 조기 종료

#### 좀비 프로세스 (defunct 프로세스) (1/2)
- 부모가 종료된 자식에 대해 wait()를 호출하지 않으면 좀비 상태가 됩니다 (ps에서 "Z" 상태로 표시됨).
- 부모가 wait() 또는 waitpid()를 호출할 때까지 좀비는 남아 있습니다.
- 이는 실행을 완료했지만 프로세스 테이블에 항목이 남아 있는 프로세스입니다.

#### 고아 프로세스 (1/2)
- 부모 프로세스가 죽으면 자식 프로세스는 고아 프로세스가 됩니다.
- init 프로세스 (pid = 1)가 고아 프로세스의 부모가 됩니다.
- init 프로세스는 주기적으로 자식을 기다리므로 결국 고아 좀비는 제거됩니다.

### 5.9 smallsh: 명령어 프로세서

#### 명령어 프로세서의 기본 로직

### 5.10 프로세스 속성

#### 프로세스 ID 0 & 1
- **스케줄러 프로세스 (swapper)**: 프로세스 ID 0
  - 이 프로세스에 해당하는 디스크의 프로그램이 없습니다.
  - 커널의 일부이며 시스템 프로세스로 알려져 있습니다.
- **init 프로세스**: 프로세스 ID 1
  - 부트스트랩 절차가 끝날 때 커널에 의해 호출됩니다.
  - 이 프로세스의 프로그램 파일은 구 버전 UNIX 시스템에서는 /etc/init였고, 최신 버전에서는 /sbin/init입니다.
  - 이 프로세스는 UNIX 시스템을 시작하는 역할을 합니다.
  - init 프로세스는 죽지 않습니다.
  - 이는 커널 내의 시스템 프로세스가 아닌 일반 사용자 프로세스이며, superuser 권한으로 실행됩니다.

#### 프로세스 그룹과 프로세스 그룹 ID (1/2)
- 프로세스 그룹은 하나 이상의 프로세스로 구성되며, 일반적으로 동일한 작업과 관련이 있습니다.
  - 프로세스는 파이프로 연결됩니다.
- 각 프로세스 그룹은 고유한 프로세스 그룹 ID를 가집니다.
- 프로세스 그룹 ID는 프로세스 ID와 유사합니다.
  - 양의 정수입니다.
  - pid_t 데이터 유형입니다.
- 각 프로세스 그룹에는 프로세스 그룹 리더가 있을 수 있습니다.
  - (pid == pgid)인 경우 프로세스 그룹 리더입니다.

#### 프로세스 그룹 변경 (1/2)
- 이 함수는 프로세스의 프로세스 ID가 pid와 같은 프로세스의 프로세스 그룹 ID를 pgid로 설정합니다.
- 프로세스는 자신이나 자식의 프로세스 그룹 ID만 설정할 수 있습니다.
- 또한 자식이 exec 함수 중 하나를 호출한 후에는 자식의 프로세스 그룹 ID를 변경할 수 없습니다.

### 세션 및 세션 ID (1/3)

- 세션은 하나 이상의 프로세스 그룹의 모음입니다.
- 세션 내의 프로세스 그룹은 하나의 포어그라운드 프로세스 그룹과 하나 이상의 백그라운드 프로세스 그룹으로 나눌 수 있습니다.
- pid가 0이면, getsid는 호출 프로세스의 세션 리더의 프로세스 그룹 ID를 반환합니다.
- 데몬 프로세스:
  - 제어 터미널이 없는 단순한 프로세스입니다.
  - 예로는 cron이 있습니다.
- 호출 프로세스가 프로세스 그룹 리더가 아니면, 이 함수는 새로운 세션을 만듭니다.
- 세 가지 일이 발생합니다:
  - 프로세스는 이 새로운 세션의 세션 리더가 됩니다. (세션 리더는 세션을 만드는 프로세스입니다.)
  - 프로세스는 새로운 프로세스 그룹의 프로세스 그룹 리더가 됩니다.
  - 프로세스는 제어 터미널이 없습니다. 프로세스가 setsid를 호출하기 전에 제어 터미널이 있었다면, 그 연결이 끊어집니다.

### 현재 작업 디렉토리 및 루트 디렉토리 (1/2)

- **현재 작업 디렉토리**:
  - 현재 작업 디렉토리는 프로세스를 시작한 fork 또는 exec을 통해 상속됩니다.
  - 프로세스별 속성입니다.
  - 자식 프로세스가 chdir을 호출하여 위치를 변경하면, 부모 프로세스의 현재 작업 디렉토리는 변경되지 않습니다.

- **현재 루트 디렉토리**:
  - 각 프로세스는 절대 경로 이름 검색에 사용되는 루트 디렉토리와도 연결됩니다.
  - 파일 시스템 계층 구조의 시작 지점입니다.
  - chroot가 성공하면, path는 '/'로 시작하는 파일 검색의 시작 지점이 됩니다.
  - 호출 프로세스에만 영향을 미치며, 시스템 전체에는 영향을 미치지 않습니다.

### 사용자 ID 및 그룹 ID 변경 (1/2)

- UNIX 시스템에서, 현재 날짜를 변경하는 등의 특권과 특정 파일을 읽거나 쓸 수 있는 접근 제어는 사용자 및 그룹 ID에 기반합니다.
- setuid 함수를 사용하여 실제 사용자 ID와 유효 사용자 ID를 설정할 수 있습니다.
- 마찬가지로, setgid 함수를 사용하여 실제 그룹 ID와 유효 그룹 ID를 설정할 수 있습니다.

### 파일 크기 제한: ulimit

- write 시스템 호출로 생성할 수 있는 파일의 크기에는 프로세스별로 제한이 있습니다.
- 현재 파일 크기 제한을 얻으려면:
  - 매개변수 cmd를 UL_GETFSIZE로 설정합니다.
- 파일 크기 제한을 변경하려면:
  - 매개변수 cmd를 UL_SETFSIZE로 설정하고 새로운 제한을 적용합니다.
- 이 방법으로 파일 크기 제한을 실제로 증가시킬 수 있는 것은 슈퍼유저만 가능합니다.
- 다른 사용자의 유효 사용자 ID를 가진 프로세스는 제한을 줄이는 것이 허용됩니다.

### 프로세스 우선순위: nice

- 시스템은 특정 프로세스에 할당되는 CPU 시간의 비율을 부분적으로 정수 nice 값에 기반하여 결정합니다.
- **nice 값 범위**:
  - 0 ~ 시스템 종속 최대값
  - 숫자가 높을수록 프로세스의 우선순위가 낮아집니다.
- 슈퍼유저 프로세스는 nice 호출의 매개변수로 음수 값을 사용하여 우선순위를 높일 수 있습니다.