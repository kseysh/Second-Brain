## Mount-on
• 디렉토리는 마운트된 파일 시스템에 의해 덮여집니다.
• 마운트 테이블 및 vfs 목록
![[Pasted image 20241008204230.png|350]]
이렇게 다른 디렉토리에 루트를 다는 행위를 mount라 한다.
## UNIX 파일 시스템
• 파일에 대한 링크 개념을 이해하려면 UNIX 파일 시스템의 구조에 대한 개념적 이해가 필요합니다.
• 오늘날 여러 UNIX 파일 시스템 구현이 사용되고 있습니다. (UFS, PCFS, HSFS 등)
![[Pasted image 20241014000136.png]]
### 부트 블록 (unix 시작시 실행하는 정보를 담음)
• UNIX가 처음 활성화될 때 사용되는 부트 코드
### 슈퍼 블록 (파일 시스템의 정보를 담음)
• 파일 시스템의 총 블록 수
• inode 자유 목록에 있는 inode 수
• 자유 블록의 비트 맵
• 블록 크기(바이트 단위) - 보통 4K
• free block 수
• used block 수
### i-노드
• 디스크에 있는 파일과 연관된 모든 inode
• 파일을 고유하게 식별함(1:1로 매핑되어 있음)
- file에 대한 정보를 가지고 있음
### 데이터 블록
• 파일 블록을 저장하기 위한 블록

## 하드 링크 & 심볼 링크
### 하드 링크
• 하드 링크는 파일에 대한 직접 포인터입니다.
• 링크 수는 i-노드를 가리키는 디렉토리 항목 수입니다.
• 링크 수가 0이 되어야 파일이 삭제된 것으로 간주한다.
• 이러한 유형의 링크를 하드 링크라고 합니다.
• 파일 시스템을 넘나들 수 없습니다. (마운트 한 파일시스템에는 하드 링크를 할 수 없다.)
• 디렉토리에 하드 링크를 생성할 수 있는 유일한 사용자는 슈퍼유저입니다.
![[Pasted image 20241008204841.png|500]]
두 파일(name1, name2)이 같은 inode에 hard link를 추가하게 되면 inode의 count는 2가 된다.
### 심볼 링크
ex) 윈도우의 바로가기 파일
• 심볼릭 링크는 파일에 대한 간접 포인터입니다.
• 심볼 링크의 실제 내용은 링크된 파일입니다.
• 파일 시스템 제한이 없습니다.
• 심볼 링크에 의해 생성된 하드 링크는 경로 문자열을 포함하는 파일에 대한 링크입니다.
![[Pasted image 20241008205609.png|500]]
첫 번째가 original 두 번째가 symbolic link이다.
symbolic link가 가리키는 inode의 block은 original의 path name를 가지고 있다. 그래서 그 path name으로 original을 찾아간다.
앞의 l은 symbolic link라는 뜻 그리고 name2 -> dirA/name1라고 dirA/name1을 가리키고 있다는 것을 볼 수 있음
## link(2) 시스템 호출
```c
#include <unistd.h>
int link(const char *orginal_path, const char *new_path);
// Returns: 0 if OK, -1 on error
```
• 새로운 디렉토리 항목을 생성하고 링크 수를 증가시킵니다. (하드 링크)
• 디렉토리에 하드 링크 생성은 슈퍼유저에게만 제한됩니다. (파일 시스템 루프 방지)
• 두 경로명이 동일한 파일 시스템에 있어야 합니다.
## unlink(2) 시스템 호출
```c
#include <unistd.h>
int unlink(const char *pathname);
// Return 0 if OK, -1 on error
```
![[Pasted image 20241008233653.png|250]]
• 기존 디렉토리 항목을 제거합니다.
• **이름이 지정된 링크만 제거**하고 파일의 링크 수를 하나 줄입니다. 
• 파일에 다른 링크가 있는 경우, 다른 링크를 통해 파일의 데이터에 여전히 접근할 수 있습니다.
• 링크 수가 0으로 줄어들면 디스크 블록이 자유 블록 목록에 추가됩니다.
unlink하려면 write permission이 있어야 하고 directory에 대해 read permission이 있어야 한다.
## remove(2) 시스템 
```c
#include <stdio.h>
int remove(const char *pathname);
// Returns: 0 if OK, -1 on error
```
• 파일의 경우, remove는 unlink와 동일합니다.
## rename(2) system call
```c
#include <stdio.h>
int rename(const char *oldname, const char *newname);
// Returns: 0 if OK, -1 on error
```
• 파일이나 디렉토리는 rename 함수를 사용하여 이름이 변경됩니다.
• **인수**
• oldname과 newname이 동일한 파일을 참조하는 경우, 함수는 아무 것도 변경하지 않고 성공적으로 반환됩니다.
## symlink(2) 시스템 호출
```c
#include <unistd.h>
int symlink(const char *realname, const char *symname);
// Returns: 0 if OK, -1 on error
```
• 심볼릭 링크 파일이 open으로 열리면, open 시스템 호출은 경로를 따라 realname을 정확하게 찾습니다. (symbolic link file을 open하면 symbolic link file이 아닌, 참조하고 있는 file을 보여준다.)
• 프로그래머가 symname 자체에 저장된 데이터를 보려면 readlink 시스템 호출을 사용해야 합니다.

## readlink(2) 시스템 호출
```c
#include <unistd.h>
ssize_t readlink(const char* sympath, char* buffer, size_t bufsize)
// Returns: number of bytes read if OK, -1 on error
```
1. sympath를 엽니다.
2. 파일의 내용을 버퍼로 읽어들입니다.
3. sympath를 닫습니다.
• 원본 파일이 제거된 경우,
	• 프로그램은 여전히 심볼릭 링크를 ‘볼’ 수 있지만, open 호출은 그 안에 포함된 경로를 따라갈 수 없으며 errno가 EEXIST로 설정된 상태로 반환됩니다.
# 파일 정보 얻기: stat과 fstat
## stat(2) 시스템 호출
```c
#include <sys/stat.h>
int stat(const char *pathname, struct stat *buf);
int fstat(int filedes, struct stat *buf);
int lstat(const char *pathname, struct stat *buf);
// All three return: 0 if OK, -1 on error
```
• 이 함수들은 파일에 대한 정보를 얻습니다. (status)
	• **stat**: 지정된 파일 (심볼릭 링크 파일에 stat을 쓰면 심볼릭 링크가 참조하고 있는 파일의 i-node 정보가 나옴, 따라서 심볼릭 링크는 lstat을 써야함)
	• **fstat**: 열린 파일
	• **lstat**: 심볼릭 링크(l이 symbolic link를 나타냄)

## 캐싱, sync 및 fsync
- 메모리에서 디스크로의 모든 전송(즉, 쓰기)은 즉시 디스크에 기록되지 않고 일반적으로 운영 체제의 데이터 공간에 캐시됩니다.
- 읽기도 캐시 내에서 버퍼링됩니다.
- UNIX는 버퍼의 데이터를 디스크에 쓰기 위한 두 가지 함수를 제공합니다.
  - **sync**: 파일 시스템에 대한 정보를 포함하는 메인 메모리 버퍼를 디스크에 플러시하는 데 사용됩니다.
  - **fsync**: 특정 파일과 관련된 모든 데이터와 속성을 플러시하는 데 호출됩니다.
## sync(2) & fsync(2) 시스템 호출
```c
#include <unistd.h>
int fsync(int filedes);
//Returns: 0 if OK, -1 on error
void sync(void);
```
- **중요한 차이점**:
  - fsync는 모든 파일 데이터가 디스크에 기록될 때까지 반환하지 않습니다.
  - sync 호출은 데이터 쓰기가 예약되었지만 완료되지 않았을 때 반환할 수 있습니다.
- UNIX 시스템은 sync를 반복적으로 호출하는 코드를 지속적으로 실행합니다.