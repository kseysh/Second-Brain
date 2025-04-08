## [[생산자와 소비자 문제]]
### producer
![[Pasted image 20250408164951.png|300]]
### consumer
![[Pasted image 20250408165007.png|300]]
### problem
![[Pasted image 20250408165112.png|300]]
더하고 저장하는 것은 여러 instruction을 사용하므로, 중간에 다른 instruction이 실행될 수 있다.
이렇게 공유 객체에 대해 race condition이 일어날 수 있다.
## Critical Section Problem
- 시스템 내에 n개의 프로세스가 있다고 가정
- 각 프로세스는 임계 구역(Critical Section)이라는 코드 구간을 가짐
	- 이 구간에서 공통 변수 수정, 테이블 업데이트, 파일 쓰기 등을 수행할 수 있음
	- 한 프로세스가 임계 구역에 있을 때, 다른 어떤 프로세스도 그 구역에 들어가면 안 됨
- 임계 구역 문제란 이러한 상황을 해결하기 위한 프로토콜을 설계하는 것
- 각 프로세스는 임계 구역에 들어가기 전에 진입 구역(entry section)에서 허락을 요청하고, 임계 구역을 마치면 종료 구역(exit section)을 거쳐 나머지 구역(remainder section)을 실행


## Critical Section Problem Solution
![[임계 영역]] 
## OS에서의 Critical section 처리
OS에서는 여러가지 race condition이 발생할 수 있음
ex) fork() system call에서 next_available_pid 가져오기
![[Pasted image 20250408170422.png|200]]
### Preemptive
- 커널 모드에서 실행 중인 프로세스도 선점될 수 있음
- 실제 사용하는 방식
### Non-preemptive
- 커널 모드에서 빠져나가거나, 차단되거나, 자발적으로 CPU를 양보할 때까지 실행함
	- 커널 모드에서는 본질적으로 race condition이 없음
## Peterson's Solution
- 두 프로세스를 대상으로 함
- load와 store가 atomic하다고 가정 (interrupt되지 않음)
- 두 프로세스는 두 개의 변수를 공유함
	- int turn
		- 임계 구역에 들어갈 차례인 프로세스
		- turn = 0: P0에게 우선순위가 있음
	- bool flag\[2]
		- 해당 프로세스가 임계 구역에 들어갈 준비가 되었는지 나타냄
		- flag\[i] = true: P<sub>i</sub>가 준비됨
		- 
- 