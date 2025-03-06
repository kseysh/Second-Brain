## OS Service
![[Pasted image 20250306171428.png|500]]
# Computer Hardware
## Computer System Architecture
입력 -> 메모리 -> 출력으로 이어진다.
모든 IO는 메모리에 적고 cpu가 메모리에 적힌 것을 읽고 출력을 요청하는 흐름으로 동작한다.
### system bus
computer system에서 주요한 컴포넌트들의 연결
- Data bus
- Command/Address bus
	- Read/Write의 두가지 타입의 트랜잭션을 가진다.
### Bus component classification
- Bus arbiter
	- 버스를 관리한다.
- Bus master
	- 요청을 보낸다.
	- ex) cpu
- Bus slave
	- 요청을 받아서 그에 맞는 행위를 한다
	- ex) memory
## I/O Operations
•	I/O 컨트롤러는 CPU의 명령에 따라 I/O 작업을 수행
•	I/O 컨트롤러의 주요 레지스터:
	•	데이터 레지스터 (입력, 출력)
	•	제어 레지스터 (제어, 상태)
•	I/O 작업은 CPU에 의해 시작됨
	•	예: 출력 작업
		1.	상태 레지스터를 읽어 출력 레지스터가 사용 가능한지 확인
		2.	사용 가능하면 출력 레지스터로 데이터 이동, 제어 레지스터에 출력 명령 전달
		3.	사용 불가능하면 이 과정을 반복하거나 대기
I/O 작업 방식
	1.	폴링(Polling) I/O vs. 인터럽트 기반(Interrupt-driven) I/O
	•	폴링 I/O: CPU가 I/O 상태 레지스터를 계속 확인하여 작업이 완료되었는지 검사
	•	인터럽트 기반 I/O: I/O 컨트롤러가 작업 완료 시 인터럽트를 발생하여 CPU에 알림
	2.	메모리 맵드(Memory-mapped) I/O vs. 포트 맵드(Port-mapped) I/O
	•	메모리 맵드 I/O: 메모리와 I/O를 동일한 주소 공간에서 관리
	•	포트 맵드 I/O: I/O 장치를 위한 별도의 주소 공간을 사용
DMA(Direct Memory Access, 직접 메모리 접근)
	•	장치 컨트롤러가 CPU의 개입 없이 버퍼 스토리지와 메인 메모리 간에 데이터 블록을 직접 전송할 수 있도록 함
	•	한 블록당 한 번의 인터럽트만 발생 (바이트당 한 번의 인터럽트 발생과 비교하여 효율적)
	•	메모리 속도에 가까운 속도로 데이터를 전송할 수 있는 고속 I/O 장치에서 사용됨