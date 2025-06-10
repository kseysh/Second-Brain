## 운영체제의 정의
Resource Allocator
- 모든 자원 관리
- 충돌하는 요청들 간의 결정을 내려 효율적이고 공정한 자원 사용 도모
Program Controller
- 프로그램 실행 제어
## 운영체제 역사
### 1950-1960
Job: 천공 카드
OS : 사람
- 사용자로부터 천공 카드 수령
- 카드를 컴퓨터에 적재
- 실행 결과 프린터로 출력
- 출력 전달
         -> 여러 작업을 배치 모니터를 이용
job to job transition을 빠르게 하기 위해 *여러 작업을 자기 테이프에 기록, 출력도 자기 테이프에 기록했다가 출력함*
#### Batch monitor
![[Pasted image 20250405150502.png|300]]
입출력 기계 (1401): 작업 배치를 테이프에 읽어들임 & 테이프에서 출력 결과 인쇄
주기계 (7094): 계산 수행 및 결과를 테이프로 저장

- SPOOL
	- Job을 카드에서 디스크로 미리 읽어두고, 출력도 디스크에 대기시켜두었다가 나중에 프린터로 출력하는 방법
	- 이로 인해 입출력 작업을 기다리지 않아도 되어 IO 성능 제약 해소
	- 작업을 큐에 넣고 비동기 처리하기 위해 사용
- Buffering
	- 입출력 장치와 CPU 사이에 데이터를 임시 저장하여 입출력 속도의 차이를 완화하는 방법
- Interrupt
	- 입출력 작업이 완료되었을 때, CPU에 신호를 보내 CPU가 불필요하게 대기하지 않도록 하는 방법

#### Multi-programmed Batch Monitor
- 여러 사용자가 시스템을 공유
- **메모리 보호** 및 **재배치** 기능이 운영체제에 추가
	- OS가 다른 Job의 메모리에 access하지 못하도록 막음 
- 여러 작업으로 인해 시스템 활용율이 향상됨
- **Concurrent Programming**이 필수로 등장함

![[Pasted image 20250405151134.png|100]]  vs ![[Pasted image 20250405151157.png|200]]
### 1960 - 1990
- OS에 파일 시스템 추가
- User는 System과 interacted함
- *Interactive time-sharing OS*
- *Response time*, *protection* 중요해짐
- PC OS
### 1990 ~ 
- Internet acess가 built in
- Multi tasking 중요
- Multimedia support

## 현재 OS의 특징
Large
Complex
Poorly understood
## 운영체제 진화
• Operator
• Batch monitor
• Multi-programmed batch monitor
• Interactive time sharing system
• PC OS
• OS with internet and multimedia