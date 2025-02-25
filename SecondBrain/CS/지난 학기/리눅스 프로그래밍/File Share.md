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

![[Pasted image 20240910160131.png|400]]
프로그램을 하나 실행하면, Process Table이 생기고 process table은 file descripter를 가지는 process table entry를 가진다. 
이 file pointer는 file table을 가져 file status flags, current file offset, v-node pointer를 사지고 v-nodepointer는 v-node와 i-node의 정보를 가진다
![[Pasted image 20240910160145.png|400]]
각각의 다른 프로세스에서 v-node를 공유할 수 있다. (같은 파일을 열면 파일과 v-node는 1대1 대응이므로)

![[Pasted image 20240910160153.png|400]]
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

![[Pasted image 20240910223852.png|300]]
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