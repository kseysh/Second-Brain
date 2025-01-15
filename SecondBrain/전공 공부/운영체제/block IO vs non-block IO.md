## block I/O
I/O 작업을 요청한 프로세스나 스레드가 요청이 완료될 때까지 block되는 IO
![[Pasted image 20250115134543.png|400]]
## non-block IO
프로세스/스레드를 블락시키지 않고 요청에 대한 현재 상태를 즉시 리턴하는 IO
![[Pasted image 20250115135220.png||400]]

### IO 작업 완료를 어떻게 확인할 것인가?
#### 완료됐는지 반복적으로 확인한다. (polling)
![[Pasted image 20250115140420.png|300]]
- 내가 읽으려고 하는 데이터가 준비되었는지 계속 read system call을 보내 확인한다.

- 단점
1. 완료된 시간과 완료를 확인한 시간 사이의 갭으로 인해 처리 속도가 느려질 수 있다.
2. 완료됐는지 반복적으로 확인하는 것은 CPU 낭비가 발생한다.
#### IO multiplexing 사용
![[Pasted image 20250115140227.png|300]]
- 관심있는 IO 작업들을 동시에 모니터링 하고 그 중에 완료된 IO 작업들을 한번에 알려준다.
###### IO multiplexing 종류
- select (잘 사용 X)
- poll (잘 사용 X)
- epoll (linux에서 사용)
- kqueue (mac에서 사용)
- IOCP (window에서 사용)
###### epoll
8개의 소켓에 대해 하나라도 read 이벤트를 받으면 알려달라고 등록한다.
소켓에 데이터가 채워지면 알려줘서 그 소켓들만 read하도록 한다.
