# POSIX:XSI Message Queue
## IPC (프로세스 간 통신)
메시지 큐는 프로세스가 다른 프로세스로부터 메시지를 보내고 받을 수 있게 해주는 POSIX:XSI 인터프로세스 통신 메커니즘입니다.
• 메시지 큐는 커널 내에 저장된 메시지의 linked list이며, 메시지 큐 식별자에 의해 식별됩니다.
```c
struct msqid_ds { /* <sys/msg.h> */
	struct ipc_perm msg_perm; /* see ch08_ipc1-_p.29_ */
	struct msg *msg_first; /* 큐에서 첫 번째 포인터 */
	struct msg *msg_last; /* 큐에서 마지막 포인터 */
	msglen_t msg_cbytes; /* 메시지 큐의 전체 사이즈 */
	msgqnum_t msg_qnum; /* # 큐에 있는 메시지 개수 */
	msglen_t msg_qbytes; /* 큐 안에 있는 전체 byte의 최대 사이즈 */
	pid_t msg_lspid; /* 가장 최근에 메시지를 send한 pid */
	pid_t msg_lrpid; /* 가장 최근에 메시지를 receive한 pid */
	time_t msg_stime; /* 가장 최근에 메시지를 send한 시간 */
	time_t msg_rtime; /* 가장 최근에 메시지를 receive한 시간 */
	time_t msg_ctime; /* 메시지가 가장 최근에 변경된 시간 */
};
```
![[Pasted image 20241126234814.png|500]]
## `msgget(2)` 시스템 호출
```c
int msgget(key_t key, int flag);
```
- `msgget` 함수는 키 매개변수에 연관된 MQ identifier를 반환합니다.
###### errno
- `EACCES`
	- 키에 해당하는 메시지 큐가 존재하지만, 권한이 거부됨
- `EEXIST`
	- 키에 해당하는 메시지 큐가 존재한다. `((flag & IPC_CREAT) && (flag & IPC_EXCL)) == -1`(key가 있어서 `IPC_EXCL`로 오류 발생)
- `ENOENT`
	- 키에 해당하는 메시지 큐가 존재하지 않다. `(msgflg & IPC_CREAT) == 0`
- `ENOSPC`
	- 시스템 전체의 메시지 큐 제한을 초과함
## `msgsnd(2)` 시스템 호출
```c
int msgsnd(int msqid, const void *ptr, size_t nbytes, int flag);
```
- `msgsnd`를 사용하여 메시지를 큐에 삽입합니다.
- 메시지는 항상 큐의 끝에 추가됩니다.
- 매개변수:
  - `ptr`: 사용자 정의 버퍼를 가리킨다. (보낼 메시지 버퍼)
```c
struct mymesg { // 이게 ptr
	long mtype; /* positive message type */
	char mtext[nbytes]; /* message data, of length nbytes */
};
```
  - `nbytes`: mtext size
  - `flag`: `IPC_NOWAIT`가 지정되지 않은 경우, 메시지를 위한 공간이 생길 때까지 대기합니다.
### `msgrcv(2)` 시스템 호출
```c
ssize_t msgrcv(int msqid, void *ptr, size_t nbytes, long type, int flag);
```
- 매개변수:
  - `ptr`: 사용자 정의 버퍼를 가리킵니다.
  - `nbytes`: 메시지 크기
  - `type` 행동: (type 번호와 일치하는 mymesg만 가져올 수 있다.)
    - `== 0`: 큐의 첫 번째 메시지 제거
    - `> 0`: 큐에서 특정 `type`의 첫 번째 메시지 제거
    - `< 0`: 절대값 이하의 가장 낮은 `type`의 첫 번째 메시지 제거
  - `flag`: `IPC_NOWAIT`, `MSG_NOERROR`
- 반환된 메시지가 `nbytes`보다 크고 `MSG_NOERROR`가 설정되어 있으면, 메시지가 잘립니다. 이 플래그가 없으면 `E2BIG` 오류가 반환됩니다.
## `msgctl(2)` 시스템 호출
```c
int msgctl(int msqid, int cmd, struct msqid_ds *buf );
```
- `msgctl`은 메시지 큐를 제거하거나 권한을 변경할 때 사용됩니다.
###### cmd
- `IPC_STAT` 
	- `msqid_ds` 데이터 구조체의 멤버를 buf에 복사합니다.
- `IPC_SET` 
	- buf로부터 `msqid_ds` 데이터 구조체의 멤버를 설정합니다.
- `IPC_RMID` 
	- 메시지 큐 `msqid`를 제거하고 해당 `msqid_ds`를 삭제합니다.
## 우선순위가 있는 메시지 큐 예제
![[Pasted image 20241127002758.png|500]]
![[Pasted image 20241127002811.png|500]]
![[Pasted image 20241127002824.png|500]]
10보다 작은 것중 가장 작은 것부터 가져온다.
### etest program
![[Pasted image 20241127002843.png|500]]
### stest program
![[Pasted image 20241127002905.png|500]]
sender는 3419로 보냈지만, receiver는 우선순위대로 1349 순서대로 받음

![[Pasted image 20241127002918.png|500]]
![[Pasted image 20241127002933.png|500]]

# 세마포어
- 세마포어는 여러 프로세스가 공유 데이터 객체에 접근할 수 있도록 하기 위한 카운터입니다.
- 1965년 E. W. Dijkstra가 상호 배제와 동기화를 관리하기 위해 제안한 추상 개념입니다.
- wait (down, P, lock)와 signal (up, V, unlock, post) 두 가지 원자적 작업을 포함하는 정수 변수입니다.
원자성 -> 하나도 실행하지 않거나, 모두 실행하는 것. 어느 하나라도 실패하면 모두 실패로 처리한다.

![[Pasted image 20241127210926.png|300]]
critical section에서는 하나의 프로세스만 실행될 수 있다.
![[Pasted image 20241127210942.png|600]]
세마포어가 0보다 작으면 세마포어 대기 큐에 들어가 있는다

## 세마포어
- POSIX:XSI 세마포어는 세마포어 요소 배열로 구성됩니다.
- 프로세스는 단일 호출로 전체 세트를 조작할 수 있습니다.

```c
struct semid_ds {
	struct ipc_perm sem_perm; // 세마포어 permission
	struct sem *sem_base; // 세마포어 set에 대한 포인터 array
	ushort_t sem_nsems; // 세마포어 개수
	time_t sem_otime; /* last-semop()time */
	time_t sem_ctime; /* last-change time */
};

struct sem {
	ushort_t semval; /* semaphore value, nonegative*/
	short sempid; /* PIDof last successful semop(), SETVAL, SETALL*/
	ushort_t semncnt; /* # awaiting semval > current value */
	ushort_t semzcnt; /* # awaiting semval = 0 */
};
```
- 각 세마포어 요소는 다음 정보를 포함합니다(`struct sem`):
  - `semval`: 세마포어 값 (0 이상)
  - `sempid`: 마지막으로 세마포어를 조작한 프로세스 ID
  - `semncnt`: 세마포어 값이 증가하기를 기다리는 프로세스 수 (중요 x)
  - `semzcnt`: 세마포어 값이 0이 되기를 기다리는 프로세스 수 (중요 x)
- 세마포어 값이 0이 되면 큐에 있는 프로세스가 깨워집니다.
- 각 세마포어 요소는 두 가지 큐를 가집니다:
  - 값 증가 대기 프로세스 큐
  - 값이 0이 되기를 기다리는 프로세스 큐 (중요 x)
![[Pasted image 20241127215700.png|500]]
## `semget(2)` 시스템 호출 (1/2)
```c
int semget(key_t key, int nsems, int flag);
```
- 키에 연관된 세마포어 식별자를 반환합니다.
- 매개변수:
  - `nsems`: 세트 내 세마포어 요소 수 (`nsems == 0`일 경우 기존 세트를 참조)
![[Pasted image 20241127215913.png|500]]
![[Pasted image 20241127215954.png|500]]
## `semctl(2)` 시스템 호출
```c
int semctl(int semid, int semnum, int cmd,…/*union semun arg*/); // 전체 세마포어에서는 semnum이 필요 x
```
- 세마포어 세트의 각 요소는 사용 전에 `semctl`로 초기화되어야 합니다.
- 매개변수:
  - `arg`: `cmd`의 값에 따라 달라집니다.
```c
union semun { // (semaphore union)
	int val; /* for SETVAL */ // 세마포어의 값
	struct semid_ds *buf; /* for IPC_STAT and IPC_SET */
	unsigned short *array; /* for GETALL and SETALL */
};
```

###### 표준 IPC 함수
• IPC_STAT: 세마포어 집합 semid의 구성원을 semid_dso에서 arg.buf로 복사합니다.
• IPC_SET: arg.buf에서 세마포어 집합의 권한을 설정합니다.
• IPC_RMID: semid로 식별된 세마포어 집합을 제거합니다.
###### 단일 세마포어 작업
(이들은 세마포어 sem_num에 적용되며, semctl이 반환하는 값들입니다)
• GETVAL: 특정 세마포어 요소의 값을 반환합니다.
• SETVAL: 특정 세마포어 요소의 값을 arg.val로 설정합니다.
###### 모든 세마포어 작업
• GETALL: arg.array에 있는 세마포어 집합의 값을 반환합니다.
• SETALL: arg.array에서 세마포어 집합의 값을 설정합니다.

![[Pasted image 20241127221123.png]]
semid가 가리키는 세마포어 셋의 특정 번호의 세마포어에 semvalue를 특정 값으로 넣어라
그래서 semun의 val을 쓰니까 arg.val에 semvalue를 넣고 semctl을 호출한다.
## `semop(2)` 시스템 호출 (1/2)
```c
int semop(int semid, struct sembuf semoparray[], size_t nops);
```
- `semop`는 사용자 정의 세마포어 작업을 세마포어 세트에 대해 원자적으로 수행합니다.
semid가 가리키고 있는 세마포어의 개수와 semoparray의 개수가 같을 필요는 없다(semid가 가리키고 있는 세마포어의 개수보다 작을 수도 있다.) 
```c
struct sembuf {
	unsigned short sem_num; /* member # in set (0, 1, ..., nsems-1) */
	short sem_op; /* operation (negative, 0, or positive) */
	short sem_flg; /* IPC_NOWAIT, SEM_UNDO */
};
```

IPC_NOWAIT가 지정된 경우, EAGAIN 오류와 함께 반환됩니다.
###### sem_op 동작
semval = resoure 개수로 생각하는 것이 일반적
`sem_op > 0`: V() -> signal operation, 세마포어를 증가시켜 자원의 해제를 기록합니다. sem_op를 semval에 더합니다.
`sem_op < 0`: P() -> wait operation, 세마포어를 감소시켜 자원의 획득을 기록합니다. semval이 abs(sem_op) 이상이 될 때까지 블록됩니다. semval은 abs(sem_op)만큼 감소합니다.
`sem_op` == 0: 세마포어가 0인지 테스트합니다. semval이 0이 될 때까지 블록됩니다. (실제 사용 x)

![[Pasted image 20241128091622.png|500]]
num에 의해 주어지는 semaphore에게 op이 지정한 operation을 지정해서 s에 저장한다.
1번 operation에 대해서는 wait를 실행한다.
N번 operation에 대해서 signal을 실행한다.
![[Pasted image 20241128091645.png|500]]

![[Pasted image 20241128091659.png|500]]
![[Pasted image 20241128091711.png|500]]
![[Pasted image 20241128091727.png|400]]
## `semop(2): SEM_UNDO` (1/4)
모든 형태의 XSI IPC 객체는 어떤 프로세스도 이를 사용하지 않을 때에도 계속 존재하기 때문에,
– 할당된 세마포어를 해제하지 않고 종료되는 프로그램에 대해 신경 써야 합니다.
– 이를 처리하기 위해 undo 기능이 있습니다.

종료 시 세마포어 조정
– 세마포어를 통해 자원이 할당된 상태에서 프로세스가 종료되면 문제가 됩니다.
– 세마포어 연산에서 SEM_UNDO 플래그를 지정하고 자원을 할당할 때(sem_op 값이 0보다 작을 때) 커널은 해당 세마포어에서 우리가 할당한 자원(sem_op의 절대값)을 기억합니다.
– 프로세스가 자발적으로 또는 비자발적으로 종료될 때, 커널은 해당 프로세스에 미결 세마포어 조정이 있는지 확인하고, 만약 있다면 해당 조정을 관련 세마포어에 적용합니다.

## Shared Memory
![[Pasted image 20241128092854.png|300]]
- 공유 메모리는 여러 프로세스가 같은 메모리 세그먼트를 읽고 쓸 수 있도록 합니다.
- 데이터가 클라이언트와 서버 사이에서 복사될 필요가 없기 때문에 가장 빠른 IPC 방식입니다.
- 서버가 공유 메모리 영역에 데이터를 기록할 때 클라이언트는 서버가 완료될 때까지 접근을 지양해야 합니다.
  - 종종 세마포어를 사용하여 공유 메모리 접근을 동기화합니다.
```c
struct shmid_ds {
	struct ipc_perm shm_perm; /* see Section 15.6.2 */
	size_t shm_segsz; /* size of segment in bytes */
	pid_t shm_lpid; /* pid of last shmop() */
	pid_t shm_cpid; /* pid of creator */
	shmatt_t shm_nattch; /* number of current attaches */
	time_t shm_atime; /* last-attach time */
	time_t shm_dtime; /* last-detach time */
	time_t shm_ctime; /* last-change time */
	. . .
};
```
## `shmget(2)` 시스템 호출
```c
int shmget(key_t key, size_t size, int flag);
```
- 매개변수:
  - `size`: 메모리 세그먼트의 최소 크기(바이트)
에러 번호 원인
• EACCES: 키에 대한 공유 메모리 식별자가 존재하지만 권한이 부여되지 않았습니다.
• EEXIST: 키에 대한 공유 메모리 식별자가 존재하지만 `((shmflg & IPC_CREAT) && (shmflg & IPC_EXCL)) != 0` 입니다.
• EINVAL: 공유 메모리 세그먼트가 생성되지 않으나 크기가 시스템에서 부과한 제한 또는 키의 세그먼트 크기와 일치하지 않습니다.
• ENOENT: 키에 대한 공유 메모리 식별자가 존재하지 않으며 (shmflg & IPC_CREAT) == 0 입니다.
• ENOSPC: 시스템 전체의 공유 메모리 식별자 제한을 초과했습니다
• ENOMEM: 지정된 공유 메모리 세그먼트를 생성할 충분한 메모리가 없습니다.
## `shmat(2)` 시스템 호출
```c
void *shmat(int shmid, const void *addr, int flag);
```
- `shmat`은 호출 프로세스의 주소 공간에 지정된 공유 메모리 세그먼트를 부착하고 `shmid`의 `shm_nattch` 값을 증가시킵니다.
- 매개변수:
  - `addr`:
    - `addr`가 0이면 커널이 선택한 첫 번째 사용 가능한 주소에 세그먼트를 부착합니다(권장 방법).
    - `addr`이 0이 아니고 `SHM_RND`가 지정되지 않은 경우 `addr`에 지정된 주소에 세그먼트를 부착합니다.
    - `addr`이 0이 아니고 `SHM_RND`가 지정된 경우 `(addr - (addr % SHMLBA))`에 세그먼트를 부착합니다.
`SHM_RND` 명령은 “반올림”을 의미합니다.
`SHMLBA`는 “낮은 경계 주소 배수”를 의미하며 항상 2의 거듭제곱입니다.
![[Pasted image 20241128093143.png|400]]
## `shmdt(2)` 시스템 호출
```c
int shmdt(void *addr);
```
- `addr` 매개변수는 이전에 `shmat` 호출로 반환된 값입니다. 호출이 성공하면, `shmdt`는 해당 `shmid_ds` 구조체의 `shm_nattch` 카운터를 감소시킵니다.

### `shmctl(2)` 시스템 호출
```c
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
```
- `cmd` 매개변수는 `shmid`로 지정된 세그먼트에 대해 수행할 다섯 가지 명령 중 하나를 지정합니다.
다음 글을 번역해드릴게요:
###### 명령어 설명
• IPC_STAT: shmid_ds를 buf로 복사합니다.
• IPC_SET: buf에 있는 값을 기반으로 공유 메모리 세그먼트 shmid의 필드 값을 설정합니다.
• IPC_RMID: 공유 메모리 세그먼트 shmid를 제거하고 해당 shmid_ds를 삭제합니다.
• SHM_LOCK: 공유 메모리 세그먼트를 메모리에 고정합니다.
• SHM_UNLOCK: 공유 메모리 세그먼트의 잠금을 해제합니다.

![[Pasted image 20241128093309.png|500]]
![[Pasted image 20241128093327.png|500]]
![[Pasted image 20241128093338.png|500]]
![[Pasted image 20241128093349.png|500]]
![[Pasted image 20241128093400.png|500]]
![[Pasted image 20241128093412.png|500]]