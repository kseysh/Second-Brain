# 7.1 파이프 (PIPE)
“프로그램은 한 가지 일을 잘하도록 작성하고, 협력하여 동작하도록 작성하며, 텍스트 스트림을 처리하도록 작성하십시오. 이것이 범용 인터페이스입니다.”
- 이는 유닉스 철학으로, 유닉스 파이프의 발명자인 **Doug McIlroy**가 제시한 개념입니다.
## 파이프 (Pipes)
- *파이프는 가장 오래된 유닉스 시스템의 IPC(프로세스 간 통신) 방식으*로, *모든 유닉스 시스템에 제공*됩니다.
- 가장 단순한 유닉스 IPC 메커니즘이며, 특수 파일로 표현됩니다.
![[Pasted image 20241126115806.png|400]]

## `pipe(2)` 시스템 호출 (1/2)
```c
int pipe(int filedes[2]); // pipe는 fd가 두개 있어야 하므로 array 사용
```
- 선입선출(FIFO) 방식으로 동작합니다.
- *`lseek`가 파이프에서 작동하지 않으므로 순서를 변경할 수 없습니다*. (lseek는 disk에서만 쓸 수 있다)
- 제한사항:
  - 하프 듀플렉스 (한 방향 통신)(양 방향으로 쓸 수는 있음)
  - 파이프는 부모와 자식 프로세스 간에 사용됩니다.(fd를 공유할 수 있는 자식은 파이프를 사용할 수 있다.)
- 인자 - `filedes`: `filedes[0]` - 읽기, `filedes[1]` - 쓰기
###### 파이프에서 `read`와 `write` 호출 시 동작 (중요)
- `read` 호출 시:
  - 파이프에 데이터가 있을 때: 즉시 반환됩니다.
  - 파이프가 비어 있을 때: 파이프에 데이터가 쓰여질 때까지 block한다.
- `write` 호출 시:
  - 파이프가 가득 차지 않았을 때: 즉시 반환됩니다.
  - 파이프가 가득 찬 경우: 공간이 생길 때까지 block 한다.
###### 파이프의 한 쪽 끝이 닫힌 경우 (중요)
- 쓰기 끝이 닫힘: 모든 데이터를 읽은 후 `read`는 파일 끝(EOF)을 나타내기 위해 0을 반환합니다.
- 읽기 끝이 닫힘: SIGPIPE 신호가 생성됩니다.
  - 이 신호를 무시하거나 잡아서 신호 처리기로부터 반환하면, `write()`는 -1을 반환하고 `errno`는 `EPIPE`로 설정됩니다.
  - pipe에 대한 write는 slow system call로 간주하기 때문에 SIGPIPE를 받으면 write()는 실패한다.

![[Pasted image 20241126122855.png|500]]
- 두 프로세스가 파이프에 대해 동시에 자유롭게 읽고 쓴다면 혼란이 발생할 것입니다.
- 이를 피하기 위해, 각 프로세스는 파이프에 대해서 읽기만 하거나 쓰기만 하고, 필요 없는 파일 디스크립터는 닫는 것이 관례입니다.
- 만약 3을 4로 변경하면, 자식 프로세스가 종료되었을 때 부모 프로세스는 여전히 파이프에 쓰기 위해 파일 디스크립터 \[4]를 열어두고 있습니다. 부모 프로세스는 추가 데이터를 기다리면서 무기한으로 블록됩니다. (deadlock 상황이 발생할 수 있다.)
- 만약 p\[1](쓰는 fd)을 close해두었다면, 파이프의 쓰기 끝이 닫힌 경우가 되며 모든 데이터를 읽고 read가 즉각 0을 반환하며 deadlock을 방지할 수 있다. (따라서 읽기/쓰기 중 자신이 사용하지 않는 fd는 close해주어야 한다.)
![[Pasted image 20241126123923.png|500]]
=> 이렇게 자신이 사용하지 않는 fd는 close해주고 사용해야 함
## 파이프 크기
- 파이프의 크기는 유한하며, POSIX에서는 512바이트로 지정하지만, 대부분의 시스템에서 이보다 크게 설정됩니다.
- 커널의 파이프 버퍼 크기: PIPE_BUF (`limits.h`)
###### 파이프가 가득 찬 경우
- `write`가 차단되어 다른 프로세스가 파이프에서 읽을 때까지 대기합니다.
###### 여러 프로세스 간의 동작
- `PIPE_BUF` 바이트 이하의 쓰기는 다른 쓰기 데이터와 interleaved되지(섞이지) 않는다. (같은 pipe에서 여러개의 프로세스가 write하더라도, 그 프로세스간 interleaved되지 않는다.)
- `PIPE_BUF` 바이트를 초과하여 쓰는 경우, 데이터가 다른 프로세스의 쓰기 데이터와 섞일 수 있다.
## Blocking & Non-Blocking
- 영원히 호출자를 차단할 수 있는 시스템 호출
  - 읽기: 데이터가 없을 때 특정 파일 타입(파이프, 터미널 장치, 네트워크 장치)에서 Block됩니다.
  - 쓰기: 데이터가 즉시 받아들여지지 않을 때 Block됩니다 (예: 파이프가 가득 찼을 때).
  - 열기: 특정 파일 타입에서 조건이 발생할 때까지 차단됩니다 (FIFO).
###### Non-Blocking I/O를 지정하는 두 가지 방법
1. 파일 디스크립터를 열 때: `O_NONBLOCK` 옵션을 지정합니다.
2. 이미 열린 디스크립터의 경우: `fcntl`을 호출하여 `O_NONBLOCK` 파일 상태 플래그를 켭니다.
## Non-Blocking write and read
- 파이프에서 `read` 또는 `write`가 차단되지 않도록 보장하는 방법:
	-  `fcntl`: `O_NONBLOCK` 플래그를 사용합니다.
  ```c
  if(fcntl(p[1], F_SETFL, O_NONBLOCK)==-1)
  perror(“fcntl”);
  ```
- 파이프가 가득 찬 경우: `fcntl(p[1], F_SETFL, O_NONBLOCK)`
	- 이후에 write 호출은 절대 블록되지 않습니다.
	- 반환 값은 -1이 되고, errno는 EAGAIN으로 설정됩니다.
- 파이프가 비어 있는 경우: `fcntl(p[0], F_SETFL, O_NONBLOCK)`
	- 이후에 read 호출은 절대 블록되지 않습니다.
	- 반환 값은 -1이 되고, errno는 EAGAIN으로 설정됩니다.
![[Pasted image 20241126130635.png|500]]
파이프가 비어있을 때의 read는 -1
파이프가 닫혔을 때의 read는 0
![[Pasted image 20241126130905.png|500]]
pipe에 4번의 write를 하는데, write는 3초에 한 번씩 read는 1초에 한 번씩 해서 3번 pipe empty가 발생하고, read가 발생한다.
parent에서 read의 반환 값이 0이면 부모도 종료한다 (자식에서의 write가 종료되었다는 의미이므로)
## `popen(3)`와 `pclose(3)` (library)
```c
FILE *popen(const char *cmdstring, const char *type);
int pclose(FILE *fp);
```
- 이 두 함수는 다음과 같은 모든 작업을 자동으로 처리합니다:
  - 파이프 생성,
  - 자식 프로세스 생성,
  - 사용하지 않는 파이프의 끝 닫기,
  - 명령 실행을 위한 셸 실행,
  - 명령 종료 대기.
- `popen` 함수는 `fork` 및 `exec`를 사용하여 `cmdstring`을 실행하고, 표준 I/O 파일 포인터를 반환합니다.
  - `type`이 "r"인 경우 파일 포인터는 `cmdstring`의 표준 출력에 연결됩니다
	  ![[Pasted image 20241126131631.png|300]]
  - `type`이 "w"인 경우 파일 포인터는 `cmdstring`의 표준 입력에 연결됩니다.
	  ![[Pasted image 20241126131648.png|300]]
![[Pasted image 20241126132105.png|500]]
## FIFO
- FIFO는 때때로 named pipe라고도 불립니다. (Disk에 file로 저장되는 pipe)
- 파이프는 관련된 프로세스 간에만 사용할 수 있으며, 공통 조상 프로세스가 파이프를 생성한 경우에만 사용할 수 있습니다.
- 그러나 FIFO를 사용하면 관계없는 프로세스도 데이터 교환이 가능합니다. (fd를 공유하지 않아도 사용할 수 있다. FIFO라는 파일에서 open만 하면 되도록 구현했기 때문)
- FIFO는 *읽기 전용 또는 쓰기 전용으로만 열 수 있습니다*. 양방향 읽기-쓰기 모드로 열 수는 없습니다 (FIFO는 하프 듀플렉스이기 때문입니다).
- stat structure의 st_mode member이다.
- 이 특성은 `S_ISFIFO` 매크로로 확인할 수 있습니다.
  ![[Pasted image 20241126163137.png|400]]
FIFO와 파이프에서 쓰기 작업은 데이터를 항상 끝에 추가하고, 읽기 작업은 항상 파이프나 FIFO의 시작에서 데이터를 반환합니다. 
파이프나 FIFO에서 `lseek`를 호출하면 `ESPIPE` 오류가 반환됩니다.
## `mkfifo(2)` 시스템 호출
```c
int mkfifo(const char *pathname, mode_t mode);
```
- FIFO는 `mkfifo`로 생성됩니다.
  - 인자: 
    - `pathname`: FIFO 파일 이름
    - `mode`: 권한 마스크
![[Pasted image 20241126165926.png|500]]
만약 O_NONBLOCK 플래그를 설정하고 read용 fd가 열리지 않았는데 write를 하면 SIGPIPE가 날아오기 때문에 file open을 항상 실패시켜야 한다.
그러나 O_NONBLOCK 플래그를 설정하고 0_RDONLY플래그를 이용해 파일을 열면, write용 fd없이 read를 하더라도 0을 리턴하는 것이지 에러가 발생하는 것은 아니므로 file open을 허용한다.
## `open()`의 비차단 플래그 (`O_NONBLOCK`)
- 기본 동작 (`O_NONBLOCK` 지정하지 않음)
  - 읽기 전용으로 열기: 다른 프로세스가 FIFO를 쓰기 위해 열 때까지 대기합니다.
  - 쓰기 전용으로 열기: 다른 프로세스가 FIFO를 읽기 위해 열 때까지 대기합니다.
- *`O_NONBLOCK`가 지정된 경우*:
  - *읽기 전용으로 열기: 즉시 파일 디스크립터를 반환합니다*.
  - *쓰기 전용으로 열기: 다른 프로세스가 FIFO를 읽기 위해 열지 않았다면 -1을 반환하고 `errno`를 `ENXIO`로 설정합니다.*
![[Pasted image 20241126171049.png|500]]
![[Pasted image 20241126171415.png|500]]
여기서 O_RDWR로 open한 이유는 O_RDONLY로 열게 되면 write용 fd가 없을 때 read를 하면 0이 반환되면서 무한 루프가 발생하게 된다. 하지만, O_RDWR로 open했다면, read가 block되면서 기다리게 된다.
# I/O Multiplexing
- 하나의 디스크립터에서 읽고 다른 디스크립터로 쓰는 작업은 block I/O를 루프 내에서 사용할 수 있습니다. 
  ```c
  while ((n = read(STDIN_FILENO, buf, BUFSIZ)) > 0)
    if (write(STDOUT_FILENO, buf, n) != n)
      err_sys("write error");
  ```
![[Pasted image 20241126174944.png|300]]
- 두 개의 디스크립터에서 읽어야 할 경우, 다른 기술이 필요합니다.
  - nonblocking I/O 모델 (폴링): CPU 시간을 낭비하므로 멀티태스킹 시스템에서는 피해야 합니다.(여러 fifo를 계속 확인하면서 write된 값이 있는지 확인해야 하기 때문)
  - multiplexing I/O 모델: `select()` 또는 `poll()`을 사용합니다.
  - 신호 기반 I/O 모델 (이벤트): `SIGIO`
  - 비동기 I/O 모델 (이벤트): `SIGIO`
## `select(2)` 시스템 호출
```c
int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
// Returns: count of ready descriptors, 0 on timeout, -1 on error
```
- `select` 함수에 전달하는 인자는 커널에 다음 정보를 제공합니다.
  - nfds: 관심 있는 디스크립터 번호를 지정합니다.
    - 예를 들어, *두 개의 파일 디스크립터 3과 4가 열려 있으면 `nfds`를 5로 설정해야 합니다.*
  - readfds: 지정된 디스크립터에서 읽기를 원하는 경우
  - writefds: 지정된 디스크립터에 쓰기를 원하는 경우
  - exceptfds: 예외 조건에 관심이 있는지 여부 (NT에서 비순차 데이터 도착)
  - timeout: 대기 시간
    - `timeout == NULL`: 무기한 대기하며, 지정된 디스크립터 중 하나가 준비되거나 신호가 잡힐 때 반환합니다.
    - `timeout->tv_sec == 0 && timeout->tv_usec == 0`: 대기하지 않고 즉시 반환하며, 모든 디스크립터를 테스트합니다.
    - `timeout->tv_sec != 0 || timeout->tv_usec != 0`: 지정된 시간 동안 대기하고, 지정된 디스크립터 중 하나가 준비되거나 시간이 만료되면 반환합니다. 디스크립터가 준비되지 않고 시간이 만료되면 반환 값은 0입니다.
```c
struct timeval {
long tv_sec; /* seconds */
long tv_usec; /* and microseconds */
};
```
-*`readfds`, `writefds`, `exceptfds`의 세 중간 인수 중 하나 또는 모두는 관심이 없을 경우 null 포인터로 지정할 수 있습니다.*
![[Pasted image 20241126182838.png|400]]

```c
int FD_ISSET(int fd, fd_set *fdset);
void FD_CLR(int fd, fd_set *fdset);// 파일 디스크립터 집합에서 제거
void FD_SET(int fd, fd_set *fdset);// 파일 디스크립터 집합에 추가
void FD_ZERO(fd_set *fdset);
```

- 첫 번째 인자는 실제로 검사할 디스크립터 번호를 나타내므로, 최대 디스크립터 번호에 1을 더해야 합니다 (디스크립터는 0부터 시작).
![[Pasted image 20241126183126.png|500]]
###### `select`의 반환 값
- 반환 값이 -1인 경우: 오류 발생 시, 예: 신호가 잡힐 때.
	- 디스크립터 세트가 수정되지 않습니다.
- 반환 값이 0인 경우: 디스크립터가 준비되지 않았을 때, 예: 타임아웃 만료 시. 
	- 모든 디스크립터 세트는 0으로 설정됩니다.
- 반환 값이 양수인 경우: 준비된 디스크립터 수를 나타내며, 세 세트에 준비된 디스크립터의 총합을 반환합니다.
  - 같은 디스크립터가 읽기 및 쓰기 준비가 되어 있으면 두 번 계산됩니다.
  - 세 개의 디스크립터 세트에서 켜진 비트는 준비된 디스크립터에 해당합니다.

![[Pasted image 20241126183934.png|500]]
pipe 3개 중에 하나가 write ready되면 select가 깨어난다.
![[Pasted image 20241126184037.png|500]]
![[Pasted image 20241126184048.png|500]]

중요한 example
com1 | com2를 했을 때 이를 shell에서 어떻게 해주는지에 대한 예제
![[Pasted image 20241126184100.png|500]]
pipe로 들어가는 것이 p\[1] pipe에서 나오는 것이 p\[2]가 된다.
`dup2(p[1],1)`로 인해 pipe로 들어가는 std out이 p\[1]이 된다.
`dup2(p[0],0)`로 인해 pipe로 들어가는 std in이 p\[0]이 된다.
![[Pasted image 20241126184113.png|500]]