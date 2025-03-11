## Common Functions of Interrupts
인터럽트(Interrupt)
	•	하드웨어 메커니즘으로, 인터럽트 벡터를 통해 Interrupt service routine(ISR)으로 제어를 전달
	•	인터럽트 벡터: 모든 서비스 루틴의 주소를 포함하는 테이블
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
	- 다음에 수행해야할 PC를 저장해둔다