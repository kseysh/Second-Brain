## File
파일은 운영 체제가 형식을 강제하지 않는다.
파일이 텍스트, 이미지등 어떤 형식이든 될 수 있다.
## File System
파일 시스템은 컴퓨터에서 파일을 저장하고, 조직화하며, 쉽게 찾고 접근할 수 있도록 도와주는 방법
파일 시스템은 사용자가 파일을 쉽게 찾고 사용할 수 있도록 구조화된 방식을 제공
파일 시스템은 데이터를 저장 장치(하드 디스크나 CD-ROM)에 저장
![[Pasted image 20240909171933.png]]

# UNIX primitives
UNIX 시스템에서 파일을 다루는 기본적인 함수들은 파일 디스크립터라는 정수를 통해 파일을 식별하고 관리하며, 이러한 함수들은 unbuffered I/O 방식으로 작동하여 데이터의 즉시 처리를 지원한다.
- **open**: 파일을 읽거나 쓰기 위해 열거나, 새 파일을 생성합니다.
- **create**: 빈 파일을 새로 만듭니다.
- **close**: 이전에 열었던 파일을 닫습니다.
- **read**: 파일에서 정보를 읽어옵니다.
- **write**: 파일에 정보를 씁니다.
- **lseek**: 파일 내에서 특정 바이트 위치로 이동합니다. 예를 들어, 파일의 특정 부분으로 커서를 옮기고 싶을 때 사용
- **unlink** / **remove**: 파일을 삭제합니다.
- **fcntl**: 파일과 관련된 속성을 제어합니다. 예를 들어, 파일 잠금을 설정하거나 파일 모드를 변경할 때 사용됩니다.
#### 특징
unix primitives는 unbuffered I/O로 데이터가 중간 버퍼에 저장되지 않고 바로 읽거나 쓰는 작업을 수행한다.
**open, read, write, lseek, close** 함수들이 unbuffered I/O를 제공하며, 이 함수들은 파일 디스크립터(file descriptors)와 함께 동작한다.




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

## `read`, `write` 에서 효율성을 높이는 방법
### buffer size 증가
system call의 횟수를 줄여 컨텍스트 스위칭을 줄일 수 있기 때문에. (하지만, 디스크에서 4K만큼 I/O가 일어남으로 무조건 커진다고 효율이 올라가지는 않는다.
(4096에서 가장 빠름))
### delayed writing
`delayed writing`: `write` system call을 하면 쓰기를 수행한 후 반환되는 것이 아닌, 커널에 버퍼 캐시로 데이터를 전송한 다음 4K씩 반환한다.
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

![[Pasted image 20240910155453.png]]
# File Share
### 프로세스 테이블
- 각 프로세스는 프로세스 테이블에 엔트리를 가지고 있다.
- 각 프로세스 테이블 엔트리 내에는 열린 파일 디스크립터의 테이블이 있다.
	- 파일 디스크립터 플래그
	- 파일 테이블 엔트리로의 포인터
### 파일 테이블
- 커널은 모든 열린 파일에 대한 파일 테이블을 유지 관리한다.
- 각 파일 테이블 엔트리는 다음을 포함한다.
	- 파일 상태 플래그 (읽기, 쓰기 등)
	- 현재 파일 offset
	- 파일의 v-node 테이블 엔트리로의 포인터
### v-node 구조체
- 각 열린 파일에는 파일의 타입과 파일에 대해 작동하는 함수에 대한 포인터를 포함하는 v-node 구조체가 있다. (v-node와 i-node와 file은 1대1로 대응됨)

![[Pasted image 20240910160131.png]]
프로그램을 하나 실행하면, Process Table이 생기고 process table은 file descripter를 가지는 process table entry를 가진다. 이 file pointer는 file table을 가져 file status flags, current file offset, v-node pointer를 사지고 v-nodepointer는 v-node와 i-node의 정보를 가진다
![[Pasted image 20240910160145.png]]
각각의 다른 프로세스에서 v-node를 공유할 수 있다. (같은 파일을 열면 파일과 v-node는 1대1대응이므로)
![[Pasted image 20240910160153.png]]
count: file table을 가리키는 fd의 개수
fd 하나당 file table은 하나 만들어짐
v-node는 파일이 디스크 안에 존재하면 계속 있음
## `dup(2)`, `dup2(2)` system call
![[Pasted image 20240910172346.png]]
`dup`, `dup2`는 기존 파일 디스크립터를 복제한다. (duplicate)
성공 시 새로운 파일 디스크립터(사용되지 않는 파일 디스크립터 중 가장 작은 값)를 반환하고, 실패 시 -1을 반환한다.
#### ex) dup
![[Pasted image 20240910222619.png]]
- dup(1)이 표준 출력 파일 디스크립터를 복제하는 예제
- 동일한 작업을 수행하기 위해 `fcntl`함수를 `F_DUPFD`와 함께 사용하는 예제 

![[Pasted image 20240910222632.png]]
file table의 count 값이 2가 된다.
프로세스 테이블에는 파일 테이블 항목을 가리키는 파일 디스크립터가 있고, 파일 테이블 항목은 v-노드 테이블 항목을 가리킨다.

#### ex) dup2
![[Pasted image 20240910222029.png]]
- “test”라는 파일을 읽기 전용 모드로 열고, 파일 디스크립터 4를 fd4에 할당. 
- dup2를 사용하여 파일 디스크립터 3을 fd4에 복제합니다. 
- **dup2는 fd4를 복제했던 파일 디스크립터를 닫는 것까지 포함한다**. 
- 동등한 함수 호출인 close(fd4)와 fcntl도 비교를 위해 나와 있다.

![[Pasted image 20240910222934.png]]
그림은 dup2 호출 전후의 프로세스 테이블 항목을 묘사한다:

• dup2 호출 전: fd4는 “test” 파일에 대한 파일 테이블 항목을 가리킵니다.
• dup2 호출 후: 원래 fd4 항목이 닫히고, fd4는 이제 fd3의 파일 테이블 항목을 복제합니다.

## `fcntl(2)` system call
이미 열려 있는 파일의 속성을 변경하는 함수 (file control)
![[Pasted image 20240910223732.png]]

• `cmd`: 프로그래머가 특정 기능을 선택하기 위해 정수 cmd 인자에 값을 설정함
• `F_DUPFD`: 기존 디스크립터 복제
• `F_GETFD`. `F_SETFD`: 파일 디스크립터 플래그 가져오기/설정하기
• `F_GETFL` , `F_SETFL`: 파일 상태 플래그 가져오기/설정하기
• `F_GETOWN` , `F_SETOWN`: 비동기 입출력 소유권 가져오기/설정하기
• `F_GETLK`, `F_SETLK` , `F_SETLKW`: 레코드 잠금 가져오기/설정하기

![[Pasted image 20240910223852.png]]
![[Pasted image 20240910223902.png|300]]
`O_ACCMODE`와 &연산을 해야 원하는 status flags를 뽑아낼 수 있는 뒤 부분을 알 수 있다.
O_ACCMODE는 read/write에 관한 정보만 알 수 있다.
## Standard input, Standard output, Standard error
![[Pasted image 20240925200123.png|400]]
자연스럽게 standard input과 standard output으로 read write를 해준다.

![[Pasted image 20240925200139.png|400]]
keyboard input을 infile로 redirection 해주는 것
여기서 오른쪽 infile은 fd다
이 때 dup2덕분에 자연스럽게 `read(0,buffer,n)`을 사용해도 infile로 read된다.

![[Pasted image 20240925200149.png|400]]
자연스럽게 standard output으로 read를 해준다.

![[Pasted image 20240925200205.png|400]]
standard output을 outfile로 redirection 해주는 것
여기서 오른쪽 outfile은 fd다
이 때 dup2덕분에 자연스럽게 `write(1,buffer,n)`을 사용해도 outfile로 write된다.

![[Pasted image 20241008184607.png]]
이렇게 하면 in과 out을 동시에 할 수 있다.
## `io`
![[Pasted image 20240925200955.png|450]]
Ctrl-D: EOF 역할을 한다.

# 표준 I/O Library
system call인 UNIX I/O는 직접 사용하기에 좋지 않기에 Standard I/O를 사용한다.
- 자동 버퍼
- 개발자 친화적 인터페이스
- 효율성에 대한 걱정을 해결함 
## `fopen(3)`
![[Pasted image 20240925202958.png|450]]
![[Pasted image 20240925203113.png|300]]
리턴 값이 파일의 포인터, pathname과 type이 문자로 들어간다.
실패시 null이 리턴된다.
## getc(3), putc(3)
![[Pasted image 20241008185516.png|500]]
## Buffering
![[Pasted image 20240925203408.png|300]]
system call을 되도록 적게 사용하기 위해서 buffering mechanism을 사용한다.

## Writing error message with `fprintf(3)`
![[Pasted image 20241008185534.png]]
restrict: fp와 format이 서로 같은 메모리 공간을 사용하면 안된다는 의미
## Error Handling
system call에서 -1이 발생하면 어떤 이유로 실패했는지를 모르게 된다.
그래서 errno에 실패한 원인에 대한 정보를 저장해두게 된다.
근데 errno는 전역변수이기 때문에 즉시 에러를 확인하지 않으면 overwrite되어 확인하기 어렵다.
errno는 새로운 시스템 콜이 만들어져도 리셋되지 않는다. (overwrite되지 않았다면)
![[Pasted image 20240925204638.png|400]]
`strerror`: errnum 값을 주면, 에러 메시지를 문자열로 말해주는 함수
`perror`: 자신의 실행 값 이름과 에러 명을 출력한다.
a.out arg1 arg2 이렇게 실행하면 main 함수에는 a.out이 argv\[0], arg1이 argv\[1], arg2가 argv\[2]가 된다.
![[Pasted image 20240925210045.png|450]]




