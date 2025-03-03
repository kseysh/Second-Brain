## 비동기 프로그래밍 != 멀티스레딩
- 비동기 프로그래밍
	- 여러 작업을 동시에 실행하는 프로그래밍 방법론
- 멀티스레딩
	- 비동기 프로그래밍의 한 종류
## 비동기 프로그래밍을 가능하게 하는 것
- multi-threads
	- 멀티 코어를 효율적으로 사용할 수 있다
	- 스레드를 너무 많이 사용하면 context switching의 비용이 든다.
	- race condition이 발생할 수 있다.
- non-block I/O
백엔드 프로그래밍의 추세는 스레드를 적게 쓰면서도 non-block IO를 통해 전체 처리량을 늘리는 방향으로 발전 중이다.


## IO 관점에서 비동기
동기 IO = block IO
비동기 IO = non-block IO

동기 IO = 요청자가 IO 완료까지 챙겨야 할 때
비동기 IO = 완료를 noti 주거나 callback으로 처리

비동기 IO = block IO를 다른 스레드에서 실행
## 백엔드 아키텍처 관점에서 비동기
하나의 서비스는 기능과 역할에 따라 여러 개의 마이크로 서비스로 구성되고 이들 사이에는 빈번하게 커뮤니케이션이 발생한다.
동기 커뮤니케이션
![[Pasted image 20250115145029.png|300]]
비동기 커뮤니케이션
![[Pasted image 20250115145103.png|300]]