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

![[Pasted image 20250408165538.png|150]]
## Critical Section Problem Solution
[[임계 영역]] 문제는 아래 세 가지 조건을 만족해야 함:
1.	상호 배제(Mutual Exclusion)
	•	프로세스 Pi가 임계 구역을 실행 중일 때, 다른 어떤 프로세스도 임계 구역을 실행할 수 없음
2.	진행(Progress)
	•	어떤 프로세스도 임계 구역에 있지 않고, 몇몇 프로세스가 임계 구역에 들어가기를 원한다면, 다음에 들어갈 프로세스의 선택이 무기한 지연되어서는 안 됨
3.	유한 대기(Bounded Waiting)
	•	한 프로세스가 임계 구역에 들어가기를 요청한 이후, 다른 프로세스들이 임계 구역에 들어갈 수 있는 횟수에는 상한이 있어야 함
