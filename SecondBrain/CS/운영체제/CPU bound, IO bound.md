## CPU burst
프로세스가 CPU에서 한 번에 연속적으로 실행되는 시간
## IO burst
프로세스가 IO 작업을 요청하고 결과를 기다리는 시간

=> 프로세스는 CPU burst와 IO burst의 연속이다.
## CPU burst 길이에 따른 빈도
![[Pasted image 20250103224539.png|300]]
대부분의 CPU burst 길이는 8ms 이하에서 끝난다.
## CPU bound 프로세스
- CPU burst가 많은 프로세스
- CPU bound 프로그램에서 적절한 스레드 수는 CPU의 수 + 1이다 - Goetz
	- 오버헤드인 context switching이 최대한 일어나지 않게 하기 위함
ex) 동영상 편집 프로그램, 머신러닝 프로그램
## IO bound 프로세스
- IO burst가 많은 프로세스
- IO bound 프로그램의 스레드 수는 여러 상황에 맞춰서 적절한 스레드 수를 찾아야 한다. (코어 개수의 1.5~3... 경험적으로 찾아야 한다.)
ex) 일반적인 백엔드 API 서버

