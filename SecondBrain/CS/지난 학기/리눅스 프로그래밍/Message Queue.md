메시지 큐는 프로세스가 다른 프로세스로부터 메시지를 보내고 받을 수 있게 해주는 [[IPC]] 통신 메커니즘입니다.
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
// Returns: 0 if OK, -1 on error
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
// Returns: size of data portion of message if OK, -1 on error
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
// Returns: 0 if OK, -1 on error
```
- `msgctl`은 메시지 큐를 제거하거나 권한을 변경할 때 사용됩니다.
###### cmd
- `IPC_STAT` 
	- `msqid_ds` 데이터 구조체의 멤버를 buf에 복사합니다.
- `IPC_SET` 
	- buf로부터 `msqid_ds` 데이터 구조체의 멤버를 설정합니다.
- `IPC_RMID` 
	- 메시지 큐 `msqid`를 제거하고 해당 `msqid_ds`를 삭제합니다.