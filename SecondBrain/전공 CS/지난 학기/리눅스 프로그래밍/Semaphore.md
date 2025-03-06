# 세마포어
- 세마포어는 여러 프로세스가 공유 데이터 객체에 접근할 수 있도록 하기 위한 카운터입니다.
- wait (down, P, lock)와 signal (up, V, unlock, post) 두 가지 원자적 작업을 포함하는 정수 변수입니다.
원자성 -> 하나도 실행하지 않거나, 모두 실행하는 것. 어느 하나라도 실패하면 모두 실패로 처리한다.
![[Pasted image 20241127210926.png|300]]
critical section에서는 하나의 프로세스만 실행될 수 있다.
![[Pasted image 20241127210942.png|600]]
세마포어가 0보다 작으면 세마포어 대기 큐에 들어가 있는다.
## 세마포어
- *세마포어는 세마포어 요소 배열로 구성*됩니다.
- *프로세스는 단일 호출로 전체 세트를 조작할 수 있습니다*.
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
	short sempid; /* PID of last successful semop(), SETVAL, SETALL*/
	ushort_t semncnt; /* # awaiting semval > current value */
	ushort_t semzcnt; /* # awaiting semval = 0 */
};
```
- 각 세마포어 요소는 다음 정보를 포함합니다(`struct sem`):
  - `semval`: 세마포어 값 (0 이상)
  - `sempid`: 마지막으로 세마포어를 조작한 프로세스 ID
  - `semncnt`: 세마포어 값이 증가하기를 기다리는 프로세스 수 - semaphore negative count(세마포어를 감소시키려는 시도가 실패한 프로세스의 개수)
  - `semzcnt`: 세마포어 값이 0이 되기를 기다리는 프로세스 수 - semaphore zero count
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

## `semop(2)` 시스템 호출 (1/2)
```c
int semop(int semid, struct sembuf semoparray[], size_t nops);
```
- `semop`는 사용자 정의 세마포어 작업을 세마포어 세트에 대해 원자적으로 수행합니다
- nops => 배열 내의 세마포어 연산의 개수입니다. 이 값은 semop 함수가 배열에서 수행할 연산의 수를 지정합니다.
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
semval = resource 개수로 생각하는 것이 일반적
`sem_op > 0`: V() -> signal operation, 세마포어를 증가시켜 자원의 해제를 기록합니다. sem_op를 semval에 더합니다.
`sem_op < 0`: P() -> wait operation, 세마포어를 감소시켜 자원의 획득을 기록합니다. semval이 abs(sem_op) 이상이 될 때까지 블록됩니다. semval은 abs(sem_op)만큼 감소합니다.
`sem_op` == 0: 세마포어가 0인지 테스트합니다. semval이 0이 될 때까지 블록됩니다. (실제 사용 x)

![[Pasted image 20241128091622.png|500]]
num에 의해 주어지는 semaphore에게 op이 지정한 operation을 지정해서 s에 저장한다.
1번 operation에 대해서는 wait를 실행한다.
N번 operation에 대해서 signal을 실행한다.

![[Pasted image 20241128091645.png|500]]

![[Pasted image 20241128091659.png|500]]
p는 wait 연산
v는 signal 연산
![[Pasted image 20241128091711.png|500]]

![[Pasted image 20241128091727.png|400]]
## `semop(2): SEM_UNDO` (1/4)
모든 형태의 XSI IPC 객체는 어떤 프로세스도 이를 사용하지 않을 때에도 계속 존재하기 때문에,
– 할당된 세마포어를 해제하지 않고 종료되는 프로그램에 대해 신경 써야 합니다.
– 이를 처리하기 위해 undo 기능이 있습니다.
if, p1이 p(s)를 실행하다 죽어버리면 p(s)를 해놓고 기다리던 p2는 deadlock에 걸린다. (p1의 v(s)를 기다리는데 이제 일어날 수 없는 일이 되었으므로)
따라서 세마포어를 wait하여 리소스를 확보해놓고, 비정상적으로 종료되면 커널이 확보한 리소스에 대한 정보를 알아두고 있다가 자동적으로 반환시켜주는 작업을 SEM_UNDO가 한다.

종료 시 세마포어 조정
– 세마포어를 통해 자원이 할당된 상태에서 프로세스가 종료되면 문제가 됩니다.
– 세마포어 연산에서 SEM_UNDO 플래그를 지정하고 자원을 할당할 때(sem_op 값이 0보다 작을 때) 커널은 해당 세마포어에서 우리가 할당한 자원(sem_op의 절대값)을 기억합니다.
– 프로세스가 자발적으로 또는 비자발적으로 종료될 때, 커널은 해당 프로세스에 끝나지 않은 세마포어 조정이 있는지 확인하고, 만약 있다면 해당 조정을 관련 세마포어에 적용합니다.
