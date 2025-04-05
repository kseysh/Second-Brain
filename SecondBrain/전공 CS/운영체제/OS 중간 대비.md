###### 운영체제의 두 가지 정의
Resource Allocator
- 모든 자원 관리
- 충돌하는 요청들 간의 결정을 내려 효율적이고 공정한 자원 사용 도모
Program Controller
- 프로그램 실행 제어
###### Batch 모니터 사용방법
카드 입출력 장치에 적재
작업 배치를 자기 테이프에 읽어들임
주기계에서 계산 수행 및 결과를 테이프로 저장
입출력 장치에서 테이프에서 출력 결과 인쇄
###### Batch Monitor가 job-to-job transition을 빠르게 할 수 있었던 방법
- SPOOL
	- Job을 카드에서 디스크로 미리 읽어두고, 출력도 디스크에 대기시켜두었다가 나중에 프린터로 출력하는 방법
	- 이로 인해 입출력 작업을 기다리지 않고 계산에 집중할 수 있어 Job-to-Job transition에서 발생하는 IO 성능 제약 해소
	- 작업을 큐에 넣고 비동기 처리하기 위해 사용
###### Buffering이란
입출력 장치와 CPU 사이에 데이터를 임시 저장하여 입출력 속도의 차이를 완화하는 방법
###### Interrupt란
입출력 작업이 완료되었을 때, CPU에 신호를 보내 CPU가 불필요하게 대기하지 않도록 하는 방법
###### Multi-programmed Batch Monitor란
- 여러 사용자가 시스템을 공유
- 메모리 보호 및 재배치 기능이 운영체제에 추가
	- OS가 다른 Job의 메모리에 access하지 못하도록 막음 
- 여러 작업으로 인해 시스템 활용율이 향상됨
- Concurrent Programming이 필수로 등장함
###### 60-90년대 OS 발전
파일 시스템 추가
Interactive time-sharing OS
###### 90~ OS 발전
- Internet acess가 built in
- Multi tasking 중요
- Multimedia support
###### 운영체제 진화
• Operator
• Batch monitor
• Multi-programmed batch monitor
• Interactive time sharing system
• PC OS
• OS with internet and multimedia
###### System bus란?
computer system에서 주요한 컴포넌트들의 연결
ex)
Data bus
Command/Address bus
###### Bus component
- Bus arbiter
	- 버스를 관리한다.
- Bus master
	- 요청을 보낸다.
	- ex) cpu
- Bus slave
	- 요청을 받아서 그에 맞는 행위를 한다
	- ex) memory
###### Polling I/O vs. Interrupt-driven I/O
- Polling I/O: CPU가 I/O 상태 레지스터를 계속 확인하여 작업이 완료되었는지 검사
- Interrupt-driven I/O: I/O 컨트롤러가 작업 완료 시 인터럽트를 발생하여 CPU에 알림
###### Memory-mapped I/O vs. Port-mapped I/O
- Memory-mapped I/O: 메모리와 I/O를 동일한 주소 공간에서 관리
- Port-mapped I/O: I/O 장치를 위한 별도의 주소 공간을 사용
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
