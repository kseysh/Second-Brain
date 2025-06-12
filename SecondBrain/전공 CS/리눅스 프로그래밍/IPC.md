## IPC (프로세스 간 통신)
- POSIX의 XSI 확장에 포함된 IPC 함수들은 System V IPC 함수에 기초하여 개발되었습니다.
- 같은 시스템 내에서 프로세스 간 정보를 공유할 수 있는 메커니즘을 제공합니다.
- message queue: pipe의 advanced 버전
- semaphores: 변수를 공유할 때 프로세스가 실행순서를 제어하는 메커니즘
- shared memory: 메모리를 공유하므로서 프로세스간 변수를 공유할 수 있게 하는 것
## 권한 구조
- IPC 객체가 생성될 때, 시스템은 IPC facility status structure도 함께 생성합니다.
![[Pasted image 20241126192335.png|500]]
owner vs creator
creator - ipc를 만든 process의 euid
owner - ipc를 만든 process의 euid지만, ipc의 owner를 다른 유저에게 넘길 때 변경될 수 있다.
- 접근 권한은 유효 사용자 ID와 그룹 ID에 의해 결정됩니다.
- IPC facility가 생성될 때 `umask` 값은 적용되지 않습니다.
- `msgctl`, `semctl`, 또는 `shmctl` 호출을 통해 `uid`, `gid`, `mode` 필드를 수정할 수 있습니다. 단, creator만 가능합니다.
## 식별자와 키
- 키 (Key):
  - IPC 객체의 외부 이름 역할을 합니다.
  - IPC 구조가 생성될 때마다(`msgget`, `semget`, `shmget` 호출) 키가 지정되어야 합니다.
  - 데이터 타입은 `key_t`이며, `<sys/types.h>`에 정의된 long integer 타입입니다.

- 식별자 (Identifier):
  - IPC 객체의 내부 이름이며, 음수가 아닌 정수입니다.
  - `get` 작업의 결과로 반환됩니다.
  - 파일 디스크립터와 비슷하게 작동하지만, *IPC 식별자는 고유*합니다. 즉, 다른 프로세스가 동일 IPC 객체를 참조할 때도 동일한 값을 사용합니다.
## `ftok(2)` 시스템 호출
```c
key_t ftok(const char *path, int id);
```
- 경로와 ID를 IPC unique한 키 값으로 변환하는 함수입니다. (file to key)
- 매개변수:
  - `path`: 기존 파일이어야 합니다.
  - `id`: 하위 8비트만 사용됩니다.
- `path`와 `id`의 조합으로 IPC 객체를 고유하게 식별할 수 있습니다.
- `path`가 존재하지 않거나 호출 프로세스에서 접근할 수 없으면 `ftok`는 -1을 반환합니다.
## IPC get 연산
```c
int msgget(key_t key, int permflags);
int semget(key_t key, int nsems, int permflags); // nsems -> number of semaphore
int shmget(key_t key, size_t size, int permflags);
```
###### 세 가지 XXXget 함수
- IPC 객체를 생성하거나 여는 함수이며 모두 IPC 키를 사용합니다.
- 키 선택 방법:
  1. 시스템이 키를 선택하게 함 (`IPC_PRIVATE`라는 상수를 key값에 넣어준다).
  2. 직접 키를 지정함. (하지만 unique하지 않은 키를 생성할 수 있어 위험)
  3. `ftok`를 호출하여 지정된 경로로부터 키를 생성함. (이것도 잘못하면 unique하지 않은 키를 생성할 수 있다.)

## IPC `ctl` operations
```c
int msgctl(int msqid, int command, struct msqid_ds *buf );
int semctl(int semid, int semnum, int command,…/*union semun arg*/);
int shmctl(int shmid, int command, struct shmid_ds *buf);
```
File의 fcntl과 stat 두 가지 작업을 한번에 하는 system call과 비슷

- IPC 리소스를 처리하기 위해 사용할 수 있는 함수들입니다.
- `IPC_STAT` 
	- 참조된 IPC 리소스 상태 정보를 반환합니다. IPC_STAT을 지정할 때, ctlsystem 호출은 반환된 정보를 저장할 적절한 타입의 구조체 포인터를 전달해야 합니다.
- `IPC_SET` 
	- IPC 리소스의 소유자, 그룹 또는 모드를 변경합니다. 또한, IPC_STAT과 마찬가지로 변경된 멤버 정보를 포함한 적절한 타입의 구조체 포인터를 전달해야 합니다.
- `IPC_RMID` 
	- IPC 리소스의 내용을 파괴하고 시스템에서 제거합니다.
