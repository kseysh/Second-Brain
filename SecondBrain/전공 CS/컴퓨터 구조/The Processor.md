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
PC: 다음번에 수행되어야 할 Instruction의 주소를 출력
## Logic Design Basics
Low voltage = 0, High voltage = 1
bit당 one wire
Multi-bit data는 multi-wire buses로 해석됨
## Combinational element
![[Pasted image 20250416213146.png|300]]
ALU의 F에는 무슨 연산인지가 들어간다.
## State(sequential) elements
- Input이 바뀌어도 output이 바로 바뀌지 않음
- 정보 저장
### Register
![[Pasted image 20250416213232.png|300]]
- 회로 내에서 데이터를 저장
	- 클럭 신호를 사용하여 저장된 값을 언제 업데이트할지 결정함
	- 에지 트리거 방식: 클럭(Clk)이 0에서 1로 변할 때 값을 업데이트함
값을 hold하고 있는 주체
### Register with write control
![[Pasted image 20250416213553.png|300]]
•	클럭 에지에서 쓰기 제어 입력이 1일 때만 값이 업데이트됨
•	저장된 값을 나중에 사용할 필요가 있을 때 사용됨
`clock rising edge && (write control == 1)`일 때만, D->Q로 update됨
## Clocking Methodology
![[Pasted image 20250416222116.png|400]]
•	조합 논리(Combinational logic)는 클럭 사이클 동안 데이터를 변환함
•	클럭 엣지(edge) 사이에서 동작함
•	상태 요소(State element)로부터 입력을 받고, 상태 요소로 출력을 보냄
•	가장 긴 지연 시간이 클럭 주기(clock period)를 결정함
## 데이터 경로(Datapath) 구성
• Datapath
	•	CPU 내에서 데이터와 주소를 처리하는 요소들
		•	레지스터, ALU, 멀티플렉서(mux), 메모리 등 포함
•	MIPS 데이터 경로를 점진적으로 구축할 예정
	•	전체 설계를 점차 구체화해 나감
## Instruction Fetch
![[Pasted image 20250416222140.png|400]]
## R-형식(R-Format) 명령어
•	두 개의 레지스터 피연산자를 읽음
•	3단계: 산술/논리 연산 수행
•	4단계: 연산 결과를 레지스터에 기록
![[Pasted image 20250416222158.png|400]]
## Load/Store 명령어
![[Pasted image 20250416222345.png|300]]
•	레지스터 피연산자를 읽음
•	3단계: 16비트 오프셋을 이용하여 주소 계산
	•	ALU 사용, 오프셋은 sign-extend 필요
•	4단계:
	•	Load: 메모리에서 읽은 값을 레지스터에 저장
	•	Store: 레지스터 값을 메모리에 저장
## Load Instruction (lw) Datapath
![[Pasted image 20250416222411.png|400]]
`lw $s1, 16($s2)`
주소 계산 $s2 + 16
메모리에서 data 읽기
읽은 data를 $s1에 저장 (쓰기)
## Store Instruction (sw) Datapath
![[Pasted image 20250416222430.png|400]]
`sw $s1, 16($s2)`
$s1 읽기
$s2 읽기
$s2+16해서 주소계산
메모리에 $s2값 쓰기
## 분기 명령어(Branch Instructions)
•	레지스터 피연산자를 읽음
•	3단계: 피연산자 비교
	•	ALU를 사용하여 빼기 연산 수행, Zero 출력 확인
•	4단계: 분기 대상 주소 계산
	•	변위를 sign-extend
	•	왼쪽으로 2비트 시프트 (워드 단위 변위)
	•	PC + 4에 더함
		•	이 계산은 이미 명령어 인출 단계에서 수행됨
## Branch Instructions
![[Pasted image 20250416222459.png|400]]
## 요소 결합(Composing the Elements)
•	초기 버전 데이터 경로는 한 클럭 사이클에 한 명령어 수행
	•	각 데이터 경로 요소는 한 번에 한 가지 기능만 수행 가능
	•	따라서 명령어 메모리와 데이터 메모리를 분리해야 함
•	**멀티플렉서(MUX)** 를 사용하여 명령어 종류에 따라 다른 데이터 소스를 선택할 수 있도록 구성
## R-Type/Load/Store Datapath
![[Pasted image 20250416222533.png|400]]
### control signal
RegWrite: 1
ALUsrc: 0
ALUop: add
MemWr: 0
MemRd: 0
MemtoReg: 0
## Full Datapath
![[Pasted image 20250416222607.png|400]]
PCSrc: branch거나, 조건이 참일 때만 1
각각 컴포넌트가 어떤 역할을 하는지...알아야하는얘기 다시 보기
