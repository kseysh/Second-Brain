## 멀티스레드의 장점
#### Responsiveness
프로세스의 일부가 Block되어도 계속 실행될 수 있다.
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
	- 자원 소유의 단위
		- 프로세스 이미지를 저장하기 위한 가상 주소 공간 할당
		- 파일, I/O 장치 등 일부 자원에 대한 제어 권한 보유
	- 실행 흐름의 단위
		- 제어 스레드를 가짐
		- 실행 상태 및 dispatch 우선 순위를 가짐
		- 프로세스 실행은 다른 프로세스와 교차되어 수행될 수 있음
- Multicore or multiprocessor 시스템에서는 병렬처리가 가능(동시성과는 다름)

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
- 실행, 준비, 차단 상태를 가진다.
- 같은 프로세스의 모든 스레드는 동일한 주소 공간을 공유하므로 suspend state가 없음
- 단일 스레드를 suspend하면, 동일한 프로세스의 모든 스레드가 일시 정지됨
- 프로세스가 종료되면 해당 프로세스 내의 모든 스레드가 종료됨
# Thread 구현
## User Thread
- user api를 통해 만들어지는 thread
- ex) java threads
## Kernel Thread
- OS에서 관리하는 thread 단위
- CPU 스케쥴링 대상이 됨
- ex) Linux, Mac OS X
# Multithreading Models
## Many-to-One
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
여러가지 스레드 모델이 모두 유저레벨에서
