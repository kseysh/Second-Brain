# exit 시스템 호출로 프로세스 종료

## 프로세스 종료
### 정상 종료
  - main 함수에서 반환
  - exit 호출
  - \_exit 또는 \_Exit 호출
### 비정상 종료
  - abort 호출
  - signal 수신
  - 마지막 스레드가 취소 요청에 응답
## `exit(3)` 
- exit의 인수는 프로세스의 종료 상태를 나타냅니다.
- 관례적으로, 프로세스는 다음을 반환합니다:
  - 정상 종료 시 0
  - 문제가 발생하면 0이 아닌 값
- `_exit`와 `_Exit`는 즉시 커널로 반환됩니다.
- exit는 특정 정리 작업을 수행합니다.
  - 표준 I/O 라이브러리의 깨끗한 종료 (fclose)
- exit(0)은 main()에서 return(0)과 동일합니다.
![[Pasted image 20241012221856.png|150]]
## `atexit(3)`
```c
#include <stdlib.h>
int atexit(void (*func)(void));
// Returns: 0 if OK, nonzero on error
```
- ISO C에서는 프로세스가 exit에 의해 자동으로 호출되는 최대 32개의 함수를 등록할 수 있습니다.
- 이러한 함수들은 exit 핸들러라고 불리며 atexit 함수를 호출하여 등록됩니다.
- exit 함수는 이 함수들을 등록된 역순으로 호출합니다.
- 플랫폼에 의해 지원되는 최대 exit 핸들러 수
![[Pasted image 20241012222342.png|500]]
exit은 하나만 exit해주는 것이 아니라 여러가지를 exit해주고 돌아온다.
`_exit`,`_Exit`은 cleanup 작업을 안하고 바로 kernel에 exit 작업을 해준다.