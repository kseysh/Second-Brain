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