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