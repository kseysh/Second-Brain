## FIFO
- FIFO는 때때로 named pipe라고도 불립니다. (Disk에 file로 저장되는 pipe)
- 파이프는 관련된 프로세스 간에만 사용할 수 있으며, 공통 조상 프로세스가 파이프를 생성한 경우에만 사용할 수 있습니다.
- 그러나 FIFO를 사용하면 관계없는 프로세스도 데이터 교환이 가능합니다. (fd를 공유하지 않아도 사용할 수 있다. FIFO라는 파일에서 open만 하면 되도록 구현했기 때문)
- FIFO는 *읽기 전용 또는 쓰기 전용으로만 열 수 있습니다*. 양방향 읽기-쓰기 모드로 열 수는 없습니다 (FIFO는 하프 듀플렉스이기 때문입니다).
- stat structure의 st_mode member이다.
- 이 특성은 `S_ISFIFO` 매크로로 확인할 수 있습니다.
  ![[Pasted image 20241126163137.png|400]]
FIFO와 파이프에서 쓰기 작업은 데이터를 항상 끝에 추가하고, 읽기 작업은 항상 파이프나 FIFO의 시작에서 데이터를 반환합니다. 
## `mkfifo(2)` 시스템 호출
```c
int mkfifo(const char *pathname, mode_t mode);
```
- FIFO는 `mkfifo`로 생성됩니다.
  - 인자: 
    - `pathname`: FIFO 파일 이름
    - `mode`: 권한 마스크

만약 O_NONBLOCK 플래그를 설정하고 read용 fd가 열리지 않았는데 write를 하면 SIGPIPE가 날아오기 때문에 file open을 항상 실패시켜야 한다.
그러나 O_NONBLOCK 플래그를 설정하고 0_RDONLY플래그를 이용해 파일을 열면, write용 fd없이 read를 하더라도 0을 리턴하는 것이지 에러가 발생하는 것은 아니므로 file open을 허용한다.
## `open()`의 non-blocking flag - (`O_NONBLOCK`)
- 기본 동작 (`O_NONBLOCK` 지정하지 않음)
  - 읽기 전용으로 열기: 다른 프로세스가 FIFO를 쓰기 위해 열 때까지 대기합니다.
  - 쓰기 전용으로 열기: 다른 프로세스가 FIFO를 읽기 위해 열 때까지 대기합니다.
- *`O_NONBLOCK`가 지정된 경우*:
  - *읽기 전용으로 열기: 즉시 파일 디스크립터를 반환합니다*.
  - *쓰기 전용으로 열기: 다른 프로세스가 FIFO를 읽기 위해 열지 않았다면 -1을 반환하고 `errno`를 `ENXIO`로 설정합니다.*
![[Pasted image 20241126171049.png|500]]
![[Pasted image 20241126171415.png|500]]
여기서 O_RDWR로 open한 이유는 O_RDONLY로 열게 되면 write용 fd가 없을 때 read를 하면 0이 반환되면서 무한 루프가 발생하게 된다. 하지만, O_RDWR로 open했다면, read가 block되면서 기다리게 된다.

![[IO Multiplexing]]