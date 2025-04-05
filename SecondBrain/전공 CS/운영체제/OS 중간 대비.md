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
###### IO 컨트롤러의 주요 레지스터
•	데이터 레지스터 (입력, 출력)
•	제어 레지스터 (제어, 상태)
###### IO 출력 작업 예시
CPU에 의해 시작됨
1.	상태 레지스터를 읽어 출력 레지스터가 사용 가능한지 확인
2.	사용 가능하면 출력 레지스터로 데이터 이동, 제어 레지스터에 출력 명령 전달
3.	사용 불가능하면 이 과정을 반복하거나 대기
###### Polling I/O vs. Interrupt-driven I/O
- Polling I/O: CPU가 I/O 상태 레지스터를 계속 확인하여 작업이 완료되었는지 검사
- Interrupt-driven I/O: I/O 컨트롤러가 작업 완료 시 인터럽트를 발생하여 CPU에 알림
###### Memory-mapped I/O vs. Port-mapped I/O
- Memory-mapped I/O: 메모리와 I/O를 동일한 주소 공간에서 관리
- Port-mapped I/O: I/O 장치를 위한 별도의 주소 공간을 사용
###### DMA란?
Direct Memory Access
- 장치 컨트롤러가 CPU의 개입 없이 버퍼 스토리지와 메인 메모리 간에 데이터 블록을 직접 전송할 수 있도록 함
- I/O 작업만 담당하는 CPU라고 생각하자
- 큰 메모리 Access를 한 번에 하는 것이 어렵기 때문에 DMA를 활용해서 옮긴다.
- Keyboard 입력 같은 경우는 데이터의 크기가 크지 않아 CPU에 그냥 인터럽트를 보낸다.
- 한 블록당 한 번의 인터럽트만 발생 (바이트당 한 번의 인터럽트 발생과 비교하여 효율적)
###### Interrupt란
하드웨어 메커니즘으로, 인터럽트 벡터를 통해 Interrupt service routine(ISR)으로 제어를 전달
OS는 interrupt driven임
###### Inerrupt vector란
모든 서비스 루틴의 주소를 포함하는 테이블
###### Trap, Exception
•	소프트웨어에 의해 생성된 인터럽트
•	에러 또는 사용자 요청(예: 오류, 강제 종료 등)에 의해 발생
###### PC란
다음 실행해야 하는 명령어의 주소를 담는 것
###### Interrupt가 들어올 때 CPU의 행동
- 현재 수행중인 instruction을 마무리 시키고, 현재 프로그램을 중지한다
- 다음에 수행해야할 PC와 현재 레지스터 값들을 저장해둔다
- Interrupt vector에서 어떤 코드를 실행해야 할지에 대한 ISR 주소를 얻어온다 
- ISR로 이동한다.
- ISR이 끝나면, 원래 하던 저장되어 있던 프로그램으로 돌아간다.
###### Important principle in designing OS
	역할과 구현의 분리
- Policy(구현): 어떤 것을 할 것인가?
- Mechanism(역할): 어떻게 동작할 것인가?
###### MS-Dos Architecture
simple structure로 이루어져 있음
모놀리식 구조여서 한 가지만 변경하더라도 전부를 컴파일해야하며, 하나가 오류가 나더라도 모두가 죽는다.
###### Layered-Approach란?
가장 안쪽은 하드웨어
가장 바깥쪽은 유저 인터페이스이다.
Layer간을 뛰어넘지 않고, 한 단계씩에만 영향을 끼칠 수 있도록 한다.
###### Microkernel (Mach)의 정의
중요하지 않은 컴포넌트는 커널에서 다 빼고 유저 레벨 프로그램에서 작성하도록 하는 방법
message 전달을 통해 사용자 모듈간 통신을 한다.
###### Microkernel (Mach)의 장점
- Kernel size가 작아진다
- 운영체제를 새로운 아키텍처로 이식하기 쉽다
- 커널 모드에서 실행되는 코드가 줄어들어 안정적이다
- 보안성이 향상된다.
###### Microkernel (Mach)의 단점
- user space와 kernel space간의 통신으로 인해 성능 오버헤드가 발생한다.
###### Program란?
디스크에 저장된 실행 가능한 파일
###### Process란?
실행 중인 프로그램을 뜻하며, 특정한 process state의 execution stream
###### process state란?
- 프로세스가 실행되는데 관여하는 모든 것들
	- Memory context
		- code, data, stack, heap
	- Hardware context
		- Program counter, CPU register, I/O register
	- System context
		- process table, open file table, page table
###### execution stream이란?
명령어가 실행되는 흐름
###### Multiprogramming이란?
- 메모리에서 여러 프로세스가 동작하는 것
- 싱글코어여도 메모리에 여러개를 올려둘 수는 있음
###### Multiprocessing이란?
- 여러개의 프로세스들이 같은 시간에 함께 동작하는 것
- 따라서 CPU가 여러개 있어야 함
###### PCB란?
Process Control Block으로
- 멀티 프로그래밍 사용시 OS가 프로세스들의 정보를 저장해두는 것
###### PCB에 저장되는 값
- 프로세스 상태
- Program counter
- Registers
- Scheduling information
- Memory management information
- I/O status information
###### Process state
- new
- running
- waiting
- ready
- terminated
###### Device queue란
I/O로 인해 waiting하는 프로세스를 모아둔 큐
###### Job queue란
시스템에서 모든 프로세스가 저장되는 큐
사용자가 프로그램 실행 시 Job queue에 들어가고, OS가 메모리 공간을 할당하면 Ready queue에 들어감
######  Short term scheduler란?
다음에 실행해야 할 프로세스를 선택하고 CPU를 할당한다.
밀리초 단위로 자주 호출되기 때문에 빠르게 동작해야 한다
###### Long term scheduler란?
- 어떤 프로세스를 Job queue에 넣을 것인지 선택하는 역할을 한다 (메모리에 너무 많은 process가 올라가면 안되므로)
- 초 또는 분 단위로 호출되기 때문에 상대적으로 느려도 괜찮다.
- multi programming 수준을 조절하는 역할을 한다.
- 적절한 프로세스 균형을 유지하는 것을 목표로 한다.
	- cpu bound 작업은 I/O bound 작업과 mix하는 것이 좋으니 그런 방식으로 조합한다.
###### build from scratch 방식
- loading 과정: code, data를 program file에서 읽어서 memory에 적재 (stack, heap은 돌아가면서 생기므로)
- create empty stack (메모리에 자리만 잡아준다.)
- PCB 만들기
- 해당 PCB를 ready queue에 넣기

###### 프로세스 복제
- 현재 state 저장(PC, register)
- memory context 복사
- PCB 복사 (pid, parent, child만 변경)
- 해당 PCB -> ready queue로 복사
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
