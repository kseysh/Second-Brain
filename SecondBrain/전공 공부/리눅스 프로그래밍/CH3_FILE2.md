## 3.1 멀티유저 환경에서의 파일들 사용자 및 소유권
### 소유자
• UNIX 시스템의 모든 파일은 시스템 사용자가 소유합니다.
• 소유자는 일반적으로 파일을 생성한 사용자입니다.
• 소유자의 실제 신원
	• 사용자 ID (종종 uid로 약칭됨)
	• 특정 사용자 이름과 연결된 uid
![[Pasted image 20241008152311.png|450]]
password는 다른 파일로 옮겨서 x로만 표기됨
• 각 UNIX 프로세스는 **일반적으로 해당 프로세스를 시작한 사용자의 uid와 연결**됩니다.
• 파일이 생성될 때, 시스템은 생성 프로세스의 uid를 참조하여 소유권을 설정합니다.
• 소유권 변경
	• 슈퍼유저 또는 파일의 소유자
	• 슈퍼유저(username=root, uid=0)
![[Pasted image 20241008191519.png]]
process를 실행한 사람의 uid가 프로세스에 기록됨 (프로세스가 shell의 uid를 상속함.)
### 그룹
• 각 사용자는 **적어도 하나 이상의 그룹**에 속해 있습니다.
```shell
$ usermod -G group1,group2 user1 // user modify user1의 그룹을 변경하는 중
$ id // 자신의 정보 출력
uid=509(user1) gid=509(group1) groups=509(group1),510(group2)
```
• `/etc/group`에 정의됨
• 그룹의 실제 신원
	• 그룹 ID (종종 gid로 약칭됨)
	• 특정 그룹 이름과 연결된 gid
• gid와 uid는 사용자가 시작한 프로세스에 의해 상속됩니다.

## 실제 사용자 및 그룹 ID
• 실제 사용자 ID (ruid) - 프로세스를 시작한 사용자의 uid
• 유효 사용자 ID (euid) - 특정 작업을 수행할 프로세스의 권한을 부여하는 데 사용됨
• 대부분의 경우, 유효 사용자 ID와 실제 사용자 ID는 일치합니다
• 유효 사용자 및 그룹 ID는 파일 접근 권한을 결정합니다.
![[Pasted image 20241008194513.png]]
euid를 이용해 /shadow파일을 변경하는 예제
## 권한 및 파일 모드
### 소유권
• 파일에 연결된 권한을 선택할 수 있음
### 권한
• 다른 사용자가 파일에 접근할 수 있는 방법을 결정함
• 슈퍼유저는 읽기, 쓰기 또는 실행 권한과 상관없이 모든 파일을 조작할 수 있음
![[Pasted image 20241008153427.png|300]]
![[Pasted image 20241008153504.png|300]]  
ls시에 owner/group/other로 permission이 나옴
![[Pasted image 20241008153612.png|400]]
이거보다 숫자가 더 편해서 쓸 일은 별로 없을거라 하심..
```shell
S_IRUSER | S_IRGRP | S_IROTH = 0444 = r--r--r--
S_IRUSR | S_IWUSR | S_IXUSR | S_IRGRP | S_IXGRP | S_IXOTH | S_IXOTH = 0755 = rwx-r-xr-x
```
앞은 무조건 S_I /W/R/X USR/GRP/OTH
### open(2)와 파일 권한
• open을 사용하여 기존 파일을 열면
• 시스템은 파일의 권한을 확인하여 프로세스에서 요청한 접근 모드가 허용되는지 확인합니다.
• 프로세스에 요청된 접근 권한이 없으면 open은 -1(errno=EACCESS)을 반환합니다.
• **파일을 열 때 커널은 유효 사용자 및 그룹 ID를 기반으로 접근 테스트를 수행**합니다.

![[Pasted image 20241008154325.png]]
ruid = usr1
euid = usr1
rgid = grp1
egid = grp1

fd1 성공
fd2 성공
fd3 EACESS (owner의 permission이 없음(뒤를 확인하지 않음))
fd4 성공 
fd4 EACESS
## 파일 접근 테스트
• 커널이 수행하는 테스트는 다음과 같습니다.
	• 프로세스의 유효 사용자 ID가 0 (슈퍼유저)인 경우 접근이 허용됩니다.
	• 프로세스의 유효 사용자 ID가 파일의 소유자 ID와 같은 경우, 적절한 사용자 접근 권한 비트가 설정되어 있으면 접근이 허용됩니다. 그렇지 않으면 접근이 거부됩니다.
	• 프로세스의 유효 그룹 ID가 파일의 그룹 ID와 같은 경우, 적절한 그룹 접근 권한 비트가 설정되어 있으면 접근이 허용됩니다. 그렇지 않으면 접근이 거부됩니다
	• 적절한 다른 접근 권한 비트가 설정되어 있으면 접근이 허용됩니다. 그렇지 않으면 접근이 거부됩니다.

## 실행 파일에 대한 추가 권한
• 파일에 실행 가능한 프로그램이 포함된 경우에만 관련이 있습니다.
![[Pasted image 20241008154918.png|300]]
원래: shell의 uid를 eid로 설정함
• `S_ISUID` 권한이 설정된 경우 file excute하면, 시스템은 생성된 프로세스에 `file owner로부터 가져온 eid를 부여`합니다.
![[Pasted image 20241008155403.png|500]]
`user $ chmod u+s a.out`
a.out에 user에다가 set user id execution을 해준다는 의미이다.
s는 x가 없는 경우에 의미가 없으므로 x를 s로 쓴다 (s가 있으면 x가 있는 것)
그래서 a.out의 euid가 file의 owner인 user2의 euid로 실행된 것.
따라서 a.out의 ruid: 실행한 사람, euid: 권한을 가진 사람

- 스티키 비트 (save-text-image)
잘 사용하지 않아 참고만 하자
• `S_ISVTX` 비트는 실행 파일에 설정할 수 있습니다.
• 초기 시스템에서는 save-text-image 비트가 파일에 설정된 경우, 실행 시 프로그램 텍스트 부분이 시스템이 중단될 때까지 시스템의 스왑 영역에 남아 있었습니다.
• 현대 UNIX 시스템에서는 이 비트가 이제 불필요합니다.
• 현대 시스템에서는 디렉토리에 대해 S_ISVTX 비트를 정의합니다.
## Password 변경 (다른 사람의 권한으로 못여는 파일을 여는 예제)
`/etc/shadow`는 owner의 read만 가능해서 직접 변경이 불가능하다.
![[Pasted image 20241008161806.png|400]]
그러나, usr/bin/passwd는 /etc/shadow를 변경할 수 있다.
![[Pasted image 20241008161801.png|400]]
euid가 0인 이유는 set user id excution이 있으므로 passwd의 euid로 shadow파일을 열었기 때문
![[Pasted image 20241008161922.png|400]]
다시 보기

## 파일 생성 마스크
• 파일 생성 마스크 (보호)
	• 파일이 생성될 때 특정 권한 비트를 자동으로 해제합니다.
	• 지정된 권한이 실수로 켜지는 것을 방지합니다.
![[Pasted image 20241008162002.png|350]]
  위의 명령어로 실행해도 자연스럽게 아래 명령어로 실행됨 (마스크가 어떻게 설정되었느냐만 다를 뿐)
## umask(2) 시스템 호출
```c
#include <sys/stat.h>
mode_t umask(mode_t cmask);
// Returns: previous file mode creation mask

$ umask // umask값 확인
$ umask -S // umask값 확인
$ umask 22 // umask값을 22로 설정
```

• umask 함수는 프로세스의 파일 모드 생성 마스크를 설정합니다.
![[Pasted image 20241008162232.png|400]]
![[Pasted image 20241008162332.png|400]]
## access(2) 시스템 호출
• `access`는 ruid(euid x) 및 그룹 ID를 기반으로 경로명의 접근 권한을 확인합니다.
• 인수
![[Pasted image 20241008202555.png|400]]
![[Pasted image 20241008202707.png|450]]

## chmod(2) 시스템 호출
```c
#include <unistd.h>
int chmod(const char *pathname, mode_t newmode);
// Returns: 0 if OK,-1 on error

// newmode에는 command의 chmod처럼 u+w 이런거는 못들어가고 777이런것만 들어갈 수 있음
```
• 기존 파일의 권한을 변경합니다. (change mode)
• 파일의 소유자(변경하려면 euid가 있어야 함) 또는 슈퍼유저만 가능합니다.
## chown(2) 시스템 호출
```c
#include <unistd.h>
int chown(const char *pathname, uid_t owner_id, gid_t group_id);
// Returns: 0 if OK,-1 on error
```
• 파일의 사용자 ID와 그룹 ID를 변경할 수 있습니다. (change owner)
• 인수
	• owner_id: 새로운 소유자 ID
	• group_id: 새로운 그룹 ID
• owner_id 또는 group_id 중 하나라도 -1인 경우, 해당 ID는 변경되지 않습니다. (안 바꾸고 싶으면 바꾸고 싶지 않은 것에 -1을 넣는다)
• 다른 owner의 파일 소유권을 변경하려는 시도 시 항상 **EPERM** 오류가 반환됩니다.
• **파일의 소유권이 변경되면 해당 파일의 set-user-id 및 set-group-id 권한이 해제**됩니다.
- 해제되지 않는다면, chown으로 다른 owner로 변경 후 그 파일을 이용해서 set-user-id로 owner가 볼 수 있는 모든 권한을 가져갈 수 있기 때문이다.

## 3.2 여러 이름을 가진 파일
파일 시스템
• 파일 시스템은 파일 및 폴더의 명명 규칙을 지정합니다.
### Mount-on
• 디렉토리는 마운트된 파일 시스템에 의해 덮여집니다.
• 마운트 테이블 및 vfs 목록
![[Pasted image 20241008204230.png|350]]
이렇게 다른 디렉토리에 루트를 다는 행위를 mount라 한다.
## UNIX 파일 시스템
• 파일에 대한 링크 개념을 이해하려면 UNIX 파일 시스템의 구조에 대한 개념적 이해가 필요합니다.
• 오늘날 여러 UNIX 파일 시스템 구현이 사용되고 있습니다. (UFS, PCFS, HSFS 등)

### 부트 블록 (unix 시작시 실행하는 정보를 담음)
• UNIX가 처음 활성화될 때 사용되는 부트 코드
### 슈퍼 블록 (파일 시스템의 정보를 담음)
• 파일 시스템의 총 블록 수
• inode 자유 목록에 있는 inode 수
• 자유 블록의 비트 맵
• 블록 크기(바이트 단위) - 보통 4K
• 자유 블록 수
• 사용된 블록 수
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
## remove(2) 시스템 호출
• 파일의 경우, remove는 unlink와 동일합니다.
## rename(2) system call
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
int symlink(const char *realname, const char *symname);
// Returns: 0 if OK, -1 on error
```
1. sympath를 엽니다.
2. 파일의 내용을 버퍼로 읽어들입니다.
3. sympath를 닫습니다.

• 원본 파일이 제거된 경우,
	• 프로그램은 여전히 심볼릭 링크를 ‘볼’ 수 있지만, open 호출은 그 안에 포함된 경로를 따라갈 수 없으며 errno가 EEXIST로 설정된 상태로 반환됩니다.
# 3.3 파일 정보 얻기: stat과 fstat
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
	dev_t st_dev; /* device number (file system) */
	dev_t st_rdev; /* device number for special files device number는 나중에 보자 */ 
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
 