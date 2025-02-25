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
-*`readfds`, `writefds`, `exceptfds`의 세 중간 인수 중 하나 또는 모두는 관심이 없을 경우 null 포인터로 지정할 수 있습니다.*

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