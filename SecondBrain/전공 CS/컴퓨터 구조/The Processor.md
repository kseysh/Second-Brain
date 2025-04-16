## Introduction
• CPU 성능 요인
	•	Instruction Count (명령어 수)
	•	ISA와 컴파일러에 의해 결정됨
	•	CPI, 클럭 주기 (Cycle Time)
	•	CPU 하드웨어에 의해 결정됨
🧮 Instruction Execution

• 모든 명령어의 공통 초기 단계
	1st step (Instruction Fetch)
	•	PC → 명령어 메모리에서 명령어 가져오기
	2nd step (Instruction Decode & Register Prefetch)
	•	명령어 해석
	•	레지스터 번호 → 레지스터 파일에서 레지스터 값 읽기
	• 3nd step (명령어 종류별 분기)
		•	ALU 사용하여 계산 수행
			•	산술 결과
			•	load/store용 메모리 주소
			•	분기 대상 주소
	•	load/store의 경우 데이터 메모리에 접근
	•	분기 또는 다음 명령어 실행을 위한 PC 업데이트
	•	PC ← target address 또는 PC + 4

⸻

필요한 경우 다이어그램이나 플로우차트도 추가로 정리해줄 수 있어!