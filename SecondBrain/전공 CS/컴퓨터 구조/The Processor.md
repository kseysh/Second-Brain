각각 컴포넌트가 어떤 역할을 하는지
연결이 되어있을 때 어떤 instruction에서 어떤 연결이 활성화되어 있는지 구분할 수 있어야 함
## Introduction
• Memory reference: lw, sw
• Arithmetic/logical: add, sub, and, or, slt
• Control transfer: beq, j
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
PC: state element
Instruction memory는 PC값을 읽고, 그에 해당하는 Instruction을 반환함
## R-형식(R-Format) 명령어
•	두 개의 레지스터 피연산자를 읽음
•	3단계: 산술/논리 연산 수행
•	4단계: 연산 결과를 레지스터에 기록
![[Pasted image 20250416222158.png|400]]
R 포맷이던, I 포맷이던 일단 두 개의 레지스터(rs, rt)를 읽는다. (쓸 지 안 쓸지는 모르지만 일단 읽는다 => Register Prefetch)
아래 두 개는 레지스터 값을 변경할 때 (Write 할 때) 사용한다.
ALU: 덧셈, 뺄셈, and, or를 한다.
read는 제어 신호가 없어도 항상 읽히지만, write는 필요할 때만 write control이 1이 된다. (RegWrite)
주소 두 개 받아서 항상 읽어오고, 쓸 때는 레지스터 번호와 데이터와 control signal을 받아 control signal이 1일 때만 해당 번호에 있는 레지스터 값을 데이터로 바뀐다.
## Load/Store 명령어
![[Pasted image 20250416222345.png|300]]
•	레지스터 피연산자를 읽음
•	3단계: 16비트 오프셋을 이용하여 주소 계산
	•	ALU 사용, 오프셋은 sign-extend 필요
•	4단계:
	•	Load: 메모리에서 읽은 값을 레지스터에 저장 (Read)
	•	Store: 레지스터 값을 메모리에 저장 (Write)
MemWrite/MemRead는 둘 중 하나만 1이어야 함
Sign extend: `lw $s0, 4($s2)` 같은 명령어를 사용할 때, $s2는 32-bit, 4는 16bit이므로 계산을 위해 sign extend를 진행해야 함
## Load Instruction (lw) Datapath
![[Pasted image 20250416222411.png|400]]
Instruction fetching 단계는 빠진 상황
`lw $s1, 16($s2)`
1. `register 1 ($s2)` , `register 2 ($s1)` 읽음 
	1. $s2는 읽어야 하는 것, $s1은 써야 하는 것 하지만, 둘 다 일단 읽긴 함
2. 주소 계산 $s2+16
	1. 16은 sign extension을 시킴
	2. 읽은 값인 $s2와 sign extension을 한 16을 더함
	3. ALU operation에는 add라는 명령어가 들어감
3. 메모리에서 data 읽기
	1. Data memory에서 MemRead 신호 1
	2. $s2+16값을 읽고, Read data로 값 출력
4. 읽은 data를 $s1에 저장
	1. write register ($s1)
	2. RegWrite 신호 1
## Store Instruction (sw) Datapath
![[Pasted image 20250416222430.png|400]]
`sw $s1, 16($s2)`
1. `register 1 ($s2)` , `register 2 ($s1)` 읽음  
	1. ($s2 값을 읽어 $s1의 메모리 어딘가에 저장해야 하므로)
	2. Read data 1에서는 $s2의 값, Read data 2에는 $s1의 값이 나옴 
2. $s2+16해서 주소계산
3. 메모리에 $s2값 쓰기
	1. Data memory에서 MemWrite 신호 1
	2. Address 부분에 Write data의 값을 저장
## 분기 명령어(Branch Instructions)
![[Pasted image 20250416222459.png|400]]
`beq $s1, $s2, label`
•	레지스터 피연산자를 읽음
•	3단계: 피연산자 비교
	• ALU를 사용하여 빼기 연산 수행, Zero 출력 확인
•	4단계: 분기 대상 주소 계산
	•	변위를 sign-extend
	•	왼쪽으로 2비트 시프트 (branch address = offset x 4이므로)
	•	PC + 4에 더함
		•	이 계산은 이미 Instruction Fetch 단계에서 수행됨
## 요소 결합(Composing the Elements)
•	초기 버전 데이터 경로는 *한 클럭 사이클에 한 명령어 수행*
	•	각 데이터 경로 요소는 한 번에 한 가지 기능만 수행 가능
	•	따라서 명령어 메모리와 데이터 메모리를 분리해야 함
•	**멀티플렉서(MUX)** 를 사용하여 명령어 종류에 따라 다른 데이터 소스를 선택할 수 있도록 구성
## R-Type/Load/Store Datapath
![[Pasted image 20250416222533.png|400]]
register write
1. R-format (ALU 결과 값을 레지스터에 반영해야 하므로)
2. lw (memory read 값(read data)을 레지스터에 반영해야 하므로)

### control signal
lw, sw, add, sub, and, or, slt, beq, j 다 한 번 해보기
`add $s1, $s2, $s3`
RegWrite: 1
ALUsrc: 0 (ALU로 들어가는 두 번째 input이 어떨 때는 sign extension 값이 쓰여야 하고 어떨 때는 register 읽은 값이 쓰여야 하므로)
ALUop: add
MemWr: 0
MemRd: 0
MemtoReg: 0 (register에 write을 하는 명령어)
## Full Datapath
![[Pasted image 20250416222607.png|400]]
PCSrc: branch거나, 조건이 참일 때만 1

## ALU Control
Load/Store: F = add
Branch: F = subtract
R-type: F depends onf 
