## Common Functions of Interrupts
인터럽트(Interrupt)
	•	하드웨어 메커니즘으로, 인터럽트 벡터를 통해 Interrupt service routine(ISR)으로 제어를 전달
	•	Interrupt Vector: 모든 서비스 루틴의 주소를 포함하는 테이블
•	인터럽트 아키텍처는 인터럽트가 발생한 명령어의 주소를 저장해야 함
•	트랩(Trap) 또는 예외(Exception):
	•	소프트웨어에 의해 생성된 인터럽트
	•	에러 또는 사용자 요청(예: 오류, 강제 종료 등)에 의해 발생
•	운영체제(OS)는 인터럽트 기반으로 동작
## Interrupt Handling
- 다음 실행해야 하는 명령어의 주소를 담는 것 : Program Counter
- OS는 CPU에 PC와 저장된 Register를 전달한다
- Interrupt가 들어오면
	- 현재 수행중인 instruction을 마무리 시키고, 현재 프로그램을 중지한다
	- 다음에 수행해야할 PC와 현재 레지스터 값들을 저장해둔다
	- Interrupt vector에서 어떤 코드를 실행해야 할지에 대한 ISR 주소를 얻어온다 
	- ISR로 이동한다.
- ISR이 끝나면, 원래 하던 저장되어 있던 프로그램으로 돌아간다.

1. 현재 명령어(instruction)를 완료
2. 현재 실행 중인 프로세스 상태 저장 (PCB 저장)
3. CPU가 커널 모드로 전환하고, 인터럽트 벡터를 이용해 ISR 주소 확인
4. ISR 실행
5. 인터럽트 처리 종료 후, 원래 실행하던 프로세스를 복원 (context restore)
6. CPU가 사용자 모드로 전환
7. P1 프로세스 실행 재개