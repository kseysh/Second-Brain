### 디렉토리
- 파일에 사용되는 많은 시스템 호출이 디렉토리를 조작하는 데 사용할 수 있습니다.
- 디렉토리와 일반 파일의 차이점:
  - 디렉토리는 open 시스템 호출을 사용하여 생성할 수 없습니다.
  - O_WRONLY 또는 O_RDWR 플래그가 설정된 상태에서 디렉토리를 열려고 하면 (errno=EISDIR) 오류가 발생합니다.
  - 오직 커널만이 디렉토리에 쓸 수 있습니다.
- 디렉토리는 포함된 각 파일이나 하위 디렉토리에 대해 하나의 디렉토리 항목으로 구성됩니다.
- 디렉토리 항목은 다음으로 구성됩니다:
  - i-노드 번호
  - 문자 필드
![[Pasted image 20241009165646.png|300]]
### 링크 및 언링크
- 각 링크는 원본과 동일한 i-노드 번호를 가진 새로운 디렉토리 슬롯과 새 이름을 생성합니다.
- unlink 시스템 호출을 사용하여 링크를 제거할 때:
  - 해당 파일명에 대한 마지막 링크를 나타내는 경우, 전체 i-노드 구조가 지워집니다.
## 디렉토리 권한
- 디렉토리 권한은 일반 파일 권한과 정확히 동일한 방식으로 구성되지만, 해석 방식이 다르다.
  - **읽기 권한**: 파일이나 하위 디렉토리의 이름 목록을 볼 수 있습니다.
  - **쓰기 권한**: 새로운 파일을 생성하거나 기존 파일을 제거할 수 있습니다.
  - **실행 권한(검색 권한)**: 디렉토리로 이동하거나 프로그램 내에서 chdir 시스템 호출을 사용할 수 있습니다.
- 예를 들어, 파일 `/usr/include/stdio.h`를 열려면:
  - `/`, `/usr`, `/usr/include`에 대해 실행 권한이 필요합니다.
- 디렉토리에 대한 읽기 권한과 실행 권한은 다른 의미를 가집니다.
  - **읽기 권한**: 디렉토리 내의 모든 파일명을 얻습니다.
  - **실행 권한**: 디렉토리를 통과하여 특정 파일명을 검색할 수 있습니다. (search permission으로 생각하자)
- 따라서 디렉토리에 대한 실행 권한 비트는 종종 검색 비트라고도 합니다.
- 저장-텍스트-이미지(sticky bit) - S_ISVTX
  - 이 비트가 디렉토리에 설정된 경우: **owner여야 한다.**
    - 다음 중 하나를 만족해야만 디렉토리에 대한 쓰기 권한으로 파일을 제거하거나 이름을 변경할 수 있습니다:
      - 파일의 owner
      - 디렉토리의 owner
      - 슈퍼유저
  - 사용자들은 다른 사람들이 소유한 파일을 삭제하거나 이름을 변경할 수 없습니다.

## dirent 구조체
```c
#include <dirent.h>
struct dirent {
	ino_t d_ino; /* i-node number */
	char d_name[NAME_MAX + 1]; /* null-terminated filename */
}
```
- d_inode의 값이 0인 것은 디렉토리에서 빈 슬롯을 나타냅니다.
## mkdir(2) 시스템 호출
```c
#include <sys/stat.h>
int mkdir(const char *pathname, mode_t mode);
// Returns: 0 if OK, -1 on error
```
- 권한은 프로세스의 umask 값에 의해 수정됩니다.
- mkdir은 새로 생성된 디렉토리에 두 개의 링크 `.`와 `..`를 추가한다는 점입니다.
- 일반 파일처럼 읽기 및 쓰기 권한만 지정하는 실수를 저지르는 경우가 많다 . 
	- 하지만 디렉토리의 경우, 디렉토리 내 파일명에 접근할 수 있도록 적어도 하나의 excute bit가 활성화되어야 한다.
![[Pasted image 20241009171718.png]]
excute permission이 없으면 cat을 통해 파일 안의 특정 파일명을 검색할 수 없다.
read permission이 없으면 ls를 통해 디렉토리 내의 모든 파일명을 얻을 수 없다
## rmdir(2) 시스템 호출
```c
#include <unistd.h>
int rmdir(const char *pathname);
// Returns: 0 if OK, -1 on error
```
- 빈 디렉토리는 rmdir 함수로 삭제됩니다. (비지 않으면 삭제 못함)
- 빈 디렉토리는 `.`와 `..` 항목만 포함하는 디렉토리입니다.
## opendir(3)
```c
#include <dirent.h>
DIR *opendir(const char *dirname);
// Returns: pointer if OK, NULL on error
```
- DIR은 표준 I/O 라이브러리에서 사용되는 FILE 타입과 유사한 방식으로 작동합니다.
- 항상 null 포인터를 테스트하는 적절한 오류 검사 코드를 작성해야 합니다.
- 프로그램이 디렉토리 접근을 마치면 닫아야 합니다. 이것은 closedir 함수로 달성할 수 있습니다.
## closedir(3)
```c
#include <dirent.h>
int closedir(DIR *dirptr);
// Returns: 0 if OK, -1 on error
```
- closedir 함수는 인수로 주어진 dirptr이 가리키는 디렉토리 스트림을 닫습니다.
## readdir(3)
```c 
#include <dirent.h>
struct dirent *readdir(DIR *dp);
// Returns: pointer if OK, NULL at end of directory or error
```
- 첫 번째 readdir에서 첫 번째 디렉토리 항목이 struct dirent로 읽힙니다. (directory entry)
- **완료 시 디렉토리 포인터는 디렉토리의 다음 항목으로 이동**합니다.
## rewinddir(3)
```c
#include <dirent.h>
void rewinddir(DIR *dp);
// Returns: 0 if OK, -1 on error
```
- rewinddir 호출 후, 다음 readdir은 dp가 가리키는 디렉토리의 첫 번째 항목을 반환합니다.

## 현재 작업 디렉토리
- 각 UNIX 프로세스는 고유한 현재 작업 디렉토리를 가집니다.
- 사용자와 관련된 현재 디렉토리는 실제로 그의 명령을 해석하는 셸 프로세스와 관련된 현재 작업 디렉토리입니다.
- 처음에는 프로세스의 현재 작업 디렉토리가 프로세스를 시작한 프로세스의 현재 작업 디렉토리로 설정됩니다. 이는 보통 셸입니다.

## chdir(2) 시스템 호출
```c
#include <unistd.h>
int chdir(const char *path);
// Return: 0 if OK, -1 on error
```
- 이 변경은 chdir을 호출하는 프로세스에만 적용됩니다. (command cd)
- **오류**
	  - 경로가 유효한 디렉토리를 정의하지 않습니다.
	  - 모든 구성 요소 디렉토리에 실행 권한이 없습니다.
- **디렉토리를 변경하고 이 새 디렉토리에 상대적인 파일명을 사용하는 것이 절대 파일명을 사용하는 것보다 더 효율적**입니다.