## 멀티스레드의 장점
#### Responsiveness
프로세스의 일부(스레드)가 Block되어도 계속 실행될 수 있다.
#### 자원 공유
스레드는 프로세스의 자원을 공유하며, shared memory나 메시지 전달 방식보다 더 쉽게 사용할 수 있다.
IPC가 필요하지 않다.
#### 경제적
- 프로세스 생성/종료보다 시간이 적게 든다
- 같은 프로세스의 thread간 변경 비용이 적다.
- Stack 과 스레드 당 static memory의 적은 리소스를 사용한다.
#### 확장성
multiprocessor(CPU 코어가 여러개) architecture의 장점을 가질 수 있다.
# Multithreaded Process Models
멀티스레드는 고려할 사항이 많아진다.
- Job 분배
- Data 분리
- Data dependency
## Parallelism
물리적으로 동일한 시간에 작동하는 것
![[Pasted image 20250324203703.png|300]]
### Types of parallelism
- Data parallelism - 데이터를 나누는 것 (하나의 일, 다른 데이터)
- Task parallelism - 일을 나누는 것 (서로 다른 일)
## Concurrency
동일한 시간에 작동하는 "것"처럼 보이는 것
![[Pasted image 20250324203651.png|300]]
## Traditional Process Model
- 하나의 process에서의 두 가지 특성
	- resource ownership의 단위
		- 프로세스 이미지를 저장하기 위한 가상 주소 공간 할당
		- 파일, I/O 장치 등 일부 자원에 대한 제어 권한 보유
	- execution stream의 단위
		- 제어 스레드를 가짐
		- 실행 상태 및 dispatch 우선 순위를 가짐
			- 프로세스 실행은 다른 프로세스와 교차되어 수행될 수 있음
- Multicore or multiprocessor 시스템에서는 병렬처리가 가능(동시성과는 다름)

![[Pasted image 20250324204849.png|250]]
- Multithread는 이 두 가지 특성을 분리한 것
	- 자원 소유의 단위는 일반적으로 Process or Task라고 한다.
	- dispatching 단위는 일반적으로 Thread라고 한다.
- stack에는 지역 변수가 저장되는데, 각 thread마다 실행 흐름이 다르므로 각 thread마다 실행하는 함수가 다르고 따라서 stack을 따로 가진다.
## Multithreading: Basics
### Thread의 특성
- 실행 상태를 가진다 (running, ready, stopped(blocked))
- 실행 중이 아닐 때, thread context를 저장한다.
- stack과 per-thread static memory를 가진다.
- 전체 메모리 공간을 공유한다.
### Thread의 상태 전이
- Running, ready, blocked 상태를 가진다.
- 같은 프로세스의 모든 스레드는 동일한 주소 공간을 공유하므로 suspend state가 없음
	- 단일 스레드를 suspend하면, 동일한 프로세스의 모든 스레드가 일시 정지됨
- 프로세스가 종료되면 해당 프로세스 내의 모든 스레드가 종료됨
# Thread 구현
## User Thread
- user api를 통해 만들어지는 thread
- ex) java threads, Pthreads, Window threads
## Kernel Thread
- OS에서 관리하는 thread 단위
- CPU 스케쥴링 대상이 됨
- ex) Linux, Mac OS X, windows, solaris, Tru64 unix
# Multithreading Models
기억할 것: One-to-One을 사용한다
## Many-to-One
 ![[Pasted image 20250325221601.png|150]]
- 여러개의 유저 스레드가 하나의 커널 스레드에 매핑되는 방식
- OS에서는 multi-thread를 제공하지 않는다.
- 유저 레벨에서 multi thread를 구현해서 사용한다.
- 하나의 thread가 Block되면, 모든 thread가 Block된다.
- 현재 시스템에서는 잘 사용하지 않음
###### Thread libray
- Thread libray가 모두 User-level library로만 제공된다.
###### Processor
CPU scheduling이 process 단위로 진행된다.
###### 특징
여러가지 스레드 기능이 모두 유저레벨에서 구현되기 때문에,
- Thread switching이 kernel mode 권한을 필요로 하지 않는다.
- 스케쥴링을 application에 맞춰서 할 수 있다.
- process state와 Thread state가 independent하다.
###### 장점
- kenel-user mode switching이 없다
- 스케쥴링을 application별로 specific하게 할 수 있다.
- OS에서 멀티스레드를 구현하지 않아도 구현할 수 있다.
###### 단점
- 병렬성을 활용하지 못한다.
## One-to-One
![[Pasted image 20250325221623.png|200]]
유저가 스레드를 하나 만들면 커널이 스레드를 하나 만드는 것을 뜻한다.
###### 특징
- 프로세스당 스레드의 수의 제약이 있다.
- user-level 스레드 라이브러리는 없지만, 커널 스레드 기능을 위한 api 제공
	- 커널 수정 필요
- 스레드 단위로 스케쥴링
###### 장점
- 병렬성 증가
- Blocking이 스레드 단위로 수행
- 커널 루틴이 멀티스레드를 지원함
###### 단점
동일한 프로세스 내에서 스레드 전환이 커널을 거치므로 성능 저하 발생(그렇게 느리지는 않아서 단점이라 하기 뭐함)
## Many-to-Many
![[Pasted image 20250325221636.png|200]]
- 여러 개의 user-level thread를 여러 개의 kernel thread에 매핑
- user-level thread : 커널은 스레드의 존재를 거의 인식하지 못함
	- user-level 스레드 생성/제거
	- 스레드 실행 스케쥴링
	- thread context 저장 및 복원
	- 스레드 간 메시지 및 데이터 전달
- kernel-level thread : user level thread에 가상 프로세서 역할 수행
	- 커널 수준 스레드 생성/제거
	- user level thread와 kernel level thread 간 매핑/언매핑
	- 프로세스 및 스레드의 컨텍스트 정보 유지
	- 스레드 간 전환
	- 스레드 스케쥴링
	- 스레드 단위의 프로세서 할당
## Thread Libraries
### Pthreads
Linux의 표준 POSIX API
![[Pasted image 20250325164205.png|400]]
![[Pasted image 20250325164358.png|400]]
#### example
1 thread
![[Pasted image 20250325164709.png|500]]
10 threads
![[Pasted image 20250325165242.png|500]]
## Implicit Threading
프로그래머보다 compiler와 run-time library가 thread의 생성 및 관리를 담당하는 것
 - 주요 방식
	 - Thread Pools (스레드 풀)
    - OpenMP
    - Grand Central Dispatch
## OpenMP
컴파일러가 알아서 확인하면 multithread를 생성 및 관리해줌
![[Pasted image 20250325170039.png|300]]
CPU core의 개수만큼 스레드를 생성해 병렬처리를 지원해준다.
## **기억할 것**
스레드의 개념 프로세스와의 차이점
(프로세스 안에 스레드가 여러개 있는 것이고, 스레드는 프로세스에서 excution stream을 여러개 분리한 것이다.)
스레드를 지원하는 OS에서는 스케쥴링 엔티티가 프로세스가 아니라 스레드 단위가 된다.
## Thread Scheduling
운영체제가 스레드를 지원할경우 프로세스가 아니라 스레드 단위로 스케쥴링이 일어난다.
아래는 설명 x