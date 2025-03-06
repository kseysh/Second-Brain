# `open(2)` system call
번호의 의미 1: command 2: system call 3: library
파일을 열거나 생성한다.
```c
#include <fcntl.h>
int open(const char *pathname, int flags, [mode_t mode]);
// Returns: file descriptor if OK, -1 on error
```
\[mode_t mode]는 옵션이라는 뜻이다.
### flag options
• `O_RDONLY` #0 읽기 전용 (Read Only)
• `O_WRONLY` #1 쓰기 전용 (writing only)
• `O_RDWR` #2 읽기 및 쓰기 전용 (read and write)
• `O_APPEND` 파일의 마지막에 쓰기 (Append to the end of file on each write.)
• `O_CREAT` 존재하지 않으면 파일 생성 (Create the file if it doesn't exist.)
	• `O_EXCL` : `O_CREAT`와 함께 사용되면, 새 파일을 만들려는 시도 중에 그 파일이 이미 존재하는 경우 오류를 발생시킨다.
• `O_TRUNC` 파일이 이미 존재할 경우, 파일의 내용을 모두 지워서 길이를 0으로 만듭니다.
• `O_NONBLOCK` 파일을 열 때 non-blocking mode로 열어, 자원이 사용 중이라도 기다리지 않고 바로 제어를 반환합니다
### mode
`O_CREAT` Flag에서만 사용된다.
File security permission을 위해 사용된다.
### `O_RDWR` flag가 필요한 이유
![[Pasted image 20240909175506.png]]
`O_RDONLY`와 `O_WRONLY`를 단순히 조합해서는 `O_RDWR`를 대체할 수 없다. 
이는 `O_RDONLY`가 0으로 정의되어 있기 때문에 비트 연산을 하면 `O_RDONLY | O_WRONLY`은 `O_WRONLY`와 동일한 값이 된다.

따라서 `O_RDWR`는 읽기/쓰기 모드를 지정하는 고유한 플래그로서 중요한 역할을 한다.

# `creat` system call
![[Pasted image 20240910152131.png]]
- 파일을 생성할 때는 주로 `open` 함수를 사용하지만 대체 방법으로서 `creat`함수를 사용할 수도 있다.
![[Pasted image 20240910152654.png]]
위 두개는 같다

## `close` system call
```C
#include <unistd.h>
int close(int filedes);
// Returns: 0 if OK, -1 on error
```
열린 파일은 `close`를 통해서만 닫을 수 있음
모든 열린 파일은 프로그램 실행이 끝나면 자동적으로 닫혀야 한다.
## `read` system call
```c
#include <unistd.h>
ssize_t read(int filedes, void *buffer, size_t n);
// Returns: number of bytes read, 0 if end of file, -1 on error
```
end of file에서 0을 반환한다.
filedes에서 n만큼 buffer에 넣고, 읽은 바이트 수를 리턴한다.
파일은 읽으면 f_offset이 알아서 증가한다. (이후 읽을 때 f_offset부터 읽는다.)

## `write(2)` system call
```c
#include <unistd.h>
ssize_t write(int filedes, const void *buffer, size_t n);
// Returns: number of bytes written if OK, -1 on error
```
`buffer`에 있는 값을 n만큼 `filedes`에 쓰고, 쓴 메모리만큼을 리턴한다.
- `write` 함수는 메모리에서 현재 파일 위치로 바이트를 복사하고, 데이터를 쓴 후 파일의 현재 위치를 업데이트한다.
- "현재 파일 위치"는 파일 내에서 다음에 데이터를 쓸 위치를 의미하며, write가 완료되면 이 위치가 자동으로 이동한다
### `write`로 이미 존재하는 파일을 쓰기 전용으로 열면
- 기존 파일에 있던 데이터는 새로운 데이터로 한 글자씩 덮어쓰게 된다. 즉, **파일의 이전 내용이 사라지고 새로운 내용이 저장**된다.

만약 파일을 열 때 `O_APPEND` 옵션을 사용했다면, 쓰기 작업이 시작되는 위치가 파일의 끝으로 자동 설정됩니다.
- 이 옵션을 사용하면 기존 파일의 내용이 지워지지 않고, 새로운 데이터가 파일 끝에 추가된다.
- 
## `read`, `write` 에서 효율성을 높이는 방법
### buffer size 증가
system call의 횟수를 줄여 컨텍스트 스위칭을 줄일 수 있기 때문에. (하지만, 디스크에서 4K만큼 I/O가 일어남으로 무조건 커진다고 효율이 올라가지는 않는다.
(4096에서 가장 빠름))
### delayed writing
`write` system call을 하면 쓰기를 수행한 후 반환되는 것이 아닌, 커널에 버퍼 캐시로 데이터를 전송한 다음 4K씩 반환한다.
그러나, 디스크에 에러가 발생하거나 커널이 멈추면 write했다고 생각한 데이터가 write되지 않았을 수도 있다.

## `lseek` system call
![[Pasted image 20240910155252.png]]
파일을 열 때 해당 파일의 offset (쓰기를 시작할 위치)을 명시적으로 설정할 수 있다.

offset: start_flag로부터의 바이트 수
• start_flag: 시작 위치
	• SEEK_SET #0: 파일의 시작 부분
	• SEEK_CUR #1: 현재 위치
	• SEEK_END #2: 파일의 끝
![[Pasted image 20240910155429.png|200]]