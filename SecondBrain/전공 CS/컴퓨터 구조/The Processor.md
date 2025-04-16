## Introduction
• CPU 성능 요인
	•	Instruction Count (명령어 수)
	•	ISA와 컴파일러에 의해 결정됨
	•	CPI, 클럭 주기 (Cycle Time)
	•	CPU 하드웨어에 의해 결정됨
## Instruction Execution
• 모든 명령어의 공통 초기 단계
1st step (Instruction Fetch)
	•	PC → 명령어 메모리에서 명령어 가져오기
2nd step (Instruction Decode & Register Prefetch(무슨 instruction인지 파악 & 미리 읽음))
	•	명령어 해석
	•	레지스터 번호 → 레지스터 파일에서 레지스터 값 읽기
• 3nd step (명령어 종류별 분기)
	•	ALU 사용하여 계산 수행
		•	산술 결과
		•	load/store용 메모리 주소 (4($s2) 이런거 계산시에 필요)
		•	분기 대상 주소(PC + offset 같은 연산시 필요)
	•	load/store의 경우 데이터 메모리에 접근
	•	PC ← target address 또는 PC + 4
## CPU Overview
![[Pasted image 20250416212845.png|400]]
Mux: wire는 함께 join될 수 없어 multiplexer를 사용한다.
## Logic Design Basics
Low voltage = 0, High voltage = 1
bit당 one wire
Multi-bit data는 multi-wire buses로 해석됨
## Combinational element

##