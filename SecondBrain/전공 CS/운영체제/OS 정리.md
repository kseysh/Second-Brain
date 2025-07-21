###### Mutex vs. Semaphore
Mutex: 임계 구역을 보호하기 위해 하나의 스레드만 임계 구역에 진입할 수 있도록 하는 장치
Semaphore: 임계 구역을 보호하기 위해 하나 이상의 스레드가 임계 구역에 진입할 수 있도록 하는 장치

busy waiting을 사용하는 방식, waiting queue를 사용하는 방식 두 가지 방식이 존재함
###### spinlock
락을 가질 수 있을 때까지 계속 반복해서 시도하는 것으로, 멀티 코어이고, 임계 영역에서의 작업이 컨텍스트 스위칭보다 빨리 끝난다면 Mutex보다 이점을 가진다.

멀티 코어여야 하는 이유: 싱글 코어라면 어차피 context switching이 발생해야 하므로 mutex보다 context switching을 덜 할 수 있다는 장점이 사라짐
###### 데드락을 만드는 네 가지 조건
- 상호 배제(Mutual exclusion)
	- 리소스를 공유해서 사용할 수 없다
- Hold and wait
	- 프로세스가 이미 하나 이상의 리소스를 취득(hold)한 상태에서 다른 프로세스가 사용하고 있는 리소스를 추가로 기다린다(wait)
- No preemption
	- 리소스 반환은 오직 그 리소스를 취득한 프로세스만 할 수 있다
- Circular wait
	- 프로세스들이 순환(circular) 형태로 서로의 리소스를 기다린다.
###### Dispatcher의 역할
스케쥴러가 선택한 프로세스를 실제로 cpu에서 실행할 수 있도록 하는 역할을 한다.
- context switching
- 커널 모드 -> 유저 모드 변경
- 새롭게 선택된 프로세스를 실행되어야 할 적절한 위치로 이동시키는 역할
###### 비동기 프로그래밍 vs. 멀티스레딩
멀티 스레딩은 비동기 프로그래밍의 한 종류로 비동기 프로그래밍은 여러 작업을 동시에 실행하는 프로그래밍 방법론
###### DMA란?
I/O 작업만 담당하는 CPU로 Device Controller가 CPU의 개입 없이 버퍼 스토리지와 메인 메모리 간에 데이터 블록을 직접 전송할 수 있도록 하는 장치
###### Interrupt mechanism
- 수행 중인 Instruction 마무리
- 다음에 수행할 PC와 레지스터 값 저장
- Interrupt vector에서 어떤 코드를 실행해야 할지에 대한 ISR 주소를 얻어온다 
- ISR를 실행한다
- 새로운 interrupt의 수신을 비활성화한다.
- ISR이 끝나면, 저장된 주소를 통해 인터럽트되었던 프로그램으로 복귀한다.
###### PCB란?
Process Control Block으로, 멀티 프로그래밍 사용시 OS가 프로세스들의 정보를 저장해두는 것
###### parallelism vs. Concurrency
parallelism: 물리적으로 동일한 시간에 작동하는 것
Concurrency: 동일한 시간에 작동하는 것처럼 보이는 것
###### test_and_set이란?
말 그대로 메모리 값을 검사하고 설정하는 특별한 원자적 하드웨어 명령어
###### swapping이란?
메모리가 부족해 디스크의 backing store에 메모리의 정보를 저장해두는 것
###### virtual memory란?
사용자 논리 메모리와 물리 메모리를 분리한 것으로, 실행을 위해 프로그램의 일부만 메모리에 존재하도록 한다.
Demand paging을 이용해 필요한 페이지만 메모리에 불러온다.
###### page fault란?
페이지 참조 시 memory에 필요한 데이터가 없다면 page fault가 발생하며 OS에 trap이 발생하고, disk에서 페이지를 swap in하고 page fault를 유발한 명령어를 재시작한다.
###### Page 교체 알고리즘
보통 LRU 알고리즘을 활용하여 page fault율을 최소화한다.
###### Thrashing이란?
프로세스가 페이지를 들여오고 내보내는 작업에만 몰두하게 되는 현상
###### 캐시 쓰기 전략
Write around: 캐시에 직접 쓰는 것이 아닌 DB에 직접 쓴다.
	- DB와 데이터 정합성 유지가 어렵다
Write Back: 캐시에 데이터를 미리 한꺼번에 써놓고 나중에 DB에 쓰기 작업을 진행
	- 캐시의 데이터 유실 문제
Write Through: 항상 캐시를 이용해 쓴다
	- 두 번의 쓰기가 항상 진행되어 성능이 낮아짐
###### 캐시 읽기 전략
Look Aside: 데이터를 찾을 때 우선 캐시에 저장된 데이터가 있는지 우선적으로 확인하는 전략
	- 캐시에 붙어있던 connection이 많았다면, 캐시가 다운된 순간 DB로 요청이 몰림
Read Through: 캐시에서만 데이터를 읽어오는 전략
	- 데이터 조회를 전적으로 캐시에만 의지하므로, redis가 다운될 경우 서비스 이용에 차질이 생길 수 있다.
###### TLB란?
Translation Look Aside Buffer로, logical 주소를 physical 주소로 변환하기 위해서 page table을 읽어야 하는데, 이 때 page table과 CPU 사이의 캐시라고 보면 된다.
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
