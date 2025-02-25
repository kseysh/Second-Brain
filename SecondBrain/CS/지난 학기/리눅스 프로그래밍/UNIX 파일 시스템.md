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
## i-노드
• 각 파일(일반 파일, 디렉토리, 특수 파일 등)은 하나의 i-노드를 사용하며 적어도 하나의 디렉토리에서 링크되어야 합니다.
• 디스크에 데이터를 가진 파일(일반 파일, 디렉토리, 심볼릭 링크)도 i-노드에서 가리키는 데이터 블록을 가집니다
• i-노드 0과 1은 사용되지 않습니다.
	• 0: “no i-node”를 의미
	• 1: 나쁜 디스크 블록(에러나는 애들)을 모으는 데 사용됨
• i-노드 2는 루트 디렉토리(/)에 예약되어 있습니다.
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
• 하드 링크와 심볼 링크에서 하드 링크가 생성됩니다.
• 하지만, 심볼 링크에 의해 생성된 하드 링크는 경로 문자열을 포함하는 파일에 대한 링크입니다.
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
![[Pasted image 20241008233945.png|450]]
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

```c
struct stat {
	mode_t st_mode; /* file type(directory of socket or fifo...) & mode (permissions) */
	ino_t st_ino; /* i-node number */ 
	dev_t st_dev; /* device number (file system) */ // file system에서 저장하는 device 파일
	dev_t st_rdev; /* device number for special files device number */ // real device 
	nlink_t st_nlink; /* 링크 count */
	uid_t st_uid; /* user ID of owner */
	gid_t st_gid; /* group ID of owner */
	off_t st_size; /* size in bytes, for regular files */
	time_t st_atime; /* time of last access */
	time_t st_mtime; /* time of last modification */
	time_t st_ctime; /* time of last file status change */
	blksize_t st_blksize; /* best I/O block size */
	blkcnt_t st_blocks; /* number of disk blocks allocated */
};
```

파일의 permission 정보를 출력하는 함수
![[Pasted image 20241008235637.png|550]]
![[Pasted image 20241008235704.png|550]]
![[Pasted image 20241008235719.png|550]]
![[Pasted image 20241008235735.png|550]]
마지막 `if (stat(name, &sb) == ―1 || sb.st_mtime ! = last)`에서 왼쪽이 true면 오른쪽을 실행하지 않는다.
![[Pasted image 20241008235754.png|550]]
변경하고 저장하지 않아도 되나봄
 