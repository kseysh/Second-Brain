# Shared Memory
![[Pasted image 20241128092854.png|300]]
- 공유 메모리는 여러 프로세스가 같은 메모리 세그먼트를 읽고 쓸 수 있도록 합니다.
- 데이터가 클라이언트와 서버 사이에서 복사될 필요가 없기 때문에 가장 빠른 IPC 방식입니다.
- 서버가 공유 메모리 영역에 데이터를 기록할 때 클라이언트는 서버가 완료될 때까지 접근을 지양해야 합니다.
  - 종종 [[Semaphore]]를 사용하여 공유 메모리 접근을 동기화합니다.
세마포어의 초기 값을 0으로 해두고, 공유 메모리 값에 값을 대입시에 signal을 통해 세마포어를 1 증가시키고, 값 사용 전에 wait를 하는 방식으로 사용하면 공유 메모리가 항상 값이 대입된 상태에서 사용할 수 있다,
```c
struct shmid_ds {
	struct ipc_perm shm_perm; /* see Section 15.6.2 */
	size_t shm_segsz; // 공유 메모리 크기
	pid_t shm_lpid; // shmop를 사용한 시간
	pid_t shm_cpid; // 공유 메모리를 만든 pid
	shmatt_t shm_nattch; // 공유 메모리와 연결되어 있는 프로세스 개수
	time_t shm_atime; // 마지막에 프로세스를 attach한 시간
	time_t shm_dtime; // 마지막에 프로세스를 detach한 시간
	time_t shm_ctime; // 마지막 프로세스를 변경한 시간 
	. . .
};
```
## `shmget(2)` 시스템 호출
```c
int shmget(key_t key, size_t size, int flag);
// return shared memory id
```
- 매개변수:
  - `size`: 메모리 세그먼트의 최소 크기(바이트)
## `shmat(2)` 시스템 호출
```c
void *shmat(int shmid, const void *addr, int flag);
// shared memory 첫 주소 리턴
```
- `shmat`은 호출 프로세스의 주소 공간에 지정된 공유 메모리 세그먼트를 부착하고 `shmid`의 `shm_nattch` 값을 증가시킵니다.
- addr = 0 => 시스템이 적절한 주소를 자동으로 선택
- flag = 0 => 공유 메모리를 RDWR로 연결

![[Pasted image 20241128093143.png|400]]
서로 가상 메모리 내에 잡혀있는 주소는 다를 수 있다.
## `shmdt(2)` 시스템 호출
```c
int shmdt(void *addr);
```
- `addr` 매개변수는 이전에 `shmat` 호출로 반환된 값입니다. 호출이 성공하면, `shmdt`는 해당 `shmid_ds` 구조체의 `shm_nattch` 카운터를 감소시킵니다.

## `shmctl(2)` 시스템 호출
```c
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
```
- `cmd` 매개변수는 `shmid`로 지정된 세그먼트에 대해 수행할 다섯 가지 명령 중 하나를 지정합니다.
###### 명령어 설명
• IPC_STAT: shmid_ds를 buf로 복사합니다.
• IPC_SET: buf에 있는 값을 기반으로 공유 메모리 세그먼트 shmid의 필드 값을 설정합니다.
• IPC_RMID: 공유 메모리 세그먼트 shmid를 제거하고 해당 shmid_ds를 삭제합니다.
• SHM_LOCK: 공유 메모리 세그먼트를 메모리에 고정합니다.
• SHM_UNLOCK: 공유 메모리 세그먼트의 잠금을 해제합니다.

![[Pasted image 20241128093309.png|500]]
![[Pasted image 20241128093327.png|500]]
세마포어 두 개를 만들고, p가 v를 기다리게 하기 위해서 세마포어를 0으로 할당한다.
![[Pasted image 20241128093338.png|500]]
read buf1
v(s1)
p(s2)
read buf2
v(s1)
p(s2)
![[Pasted image 20241128093349.png|500]]
p(s1)
v(s2)
write buf1
p(s1)
v(s2)
write buf2
![[Pasted image 20241128093400.png|500]]
![[Pasted image 20241128093412.png|500]]
read(buf2)와 write(buf1)은 parallel하게 실행이 가능하다.
read(buf1)이 실행되기 전에 write(buf1)이 먼저 실행되어야 하는데, 그것을 제어하기 위해 read(buf2)후에 v1을 두어 기다리게 하고, write(buf1)이 끝나면 p1을 하여 write 이후에 read가 실행될 수 있도록 한다.