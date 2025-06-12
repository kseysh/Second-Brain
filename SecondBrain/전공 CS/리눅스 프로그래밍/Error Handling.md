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




