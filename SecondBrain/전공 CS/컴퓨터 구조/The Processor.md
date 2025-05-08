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
R-type: F depends on funct field

opcode에서 파생된 2비트 ALUOp을 가정합니다 
Combinational logic derives ALU control
![[Pasted image 20250429231457.png|400]]
## The Main Control Unit
Control signals derived from instruction
![[Pasted image 20250501150833.png|400]]
## Datapath with Control
![[Pasted image 20250501150938.png|500]]
## R-Type Instruction
![[Pasted image 20250501151055.png|500]]
## Load Instruction
![[Pasted image 20250501151112.png|500]]
## Branch on Equal Instruction
![[Pasted image 20250501151134.png|500]]
## Datapath with Jumps Added
target PC = Top 4 bits of old PC +26-bit jump address + 00
![[Pasted image 20250501151205.png|500]]
## Performance Issues
•	가장 긴 지연 시간이 클럭 주기를 결정함
	•	Critical path: load 명령어
	•	명령어 메모리 → 레지스터 파일 → ALU → 데이터 메모리 → 레지스터 파일
•	명령어마다 주기를 다르게 설정하는 것은 현실적이지 않음
	•	이는 일반적인 경우(common case)를 빠르게 처리하라는 설계 원칙을 위반함
•	우리는 파이프라이닝을 통해 성능을 향상시킬 것임
## 파이프라이닝 비유
![[Pasted image 20250501151639.png|300]]
•	파이프라인 방식의 세탁: 실행을 겹쳐서 수행
	•	병렬성이 성능을 향상시킴
•	세탁물 4개 처리:
	•	속도 향상 = 8 / 3.5 = 2.3
•	멈추지 않고 계속 실행할 경우:
	•	속도 향상 = 2n / (0.5n + 1.5) ≈ 4 = 단계 수
## MIPS 파이프라인
•	5단계, 각 단계마다 한 스텝 수행
	•	**IF**: 메모리에서 명령어 가져오기
	•	**ID**: 명령어 디코딩 및 레지스터 읽기
	•	**EX**: 연산 수행 또는 주소 계산 (ALU)
	•	**MEM**: 메모리 피연산자 접근
	•	**WB**: 결과를 레지스터에 저장
## 파이프라인 성능
•	각 단계의 소요 시간 가정
	•	레지스터 읽기/쓰기: 100ps
	•	나머지 단계: 200ps
•	단일 사이클 데이터 경로와 파이프라인 데이터 경로 비교
![[Pasted image 20250501151833.png|400]]
![[Pasted image 20250501151850.png|400]]

## Pipeline Speedup
- 모든 단계가 균형 잡혀 있으면
	- 즉, 모든 단계가 동일한 시간이 소요된다면
	- ![[Pasted image 20250501152016.png|200]]
•	If not balanced, speedup is less
•	속도 향상은 처리량 증가로 인해 발생
	•	지연 시간(각 명령어의 실행 시간)은 감소하지 않음
## 파이프라이닝과 ISA 설계
•	MIPS ISA는 파이프라이닝을 고려하여 설계됨
	•	모든 명령어는 32비트
		•	한 사이클 내에서 가져오고 디코딩하기 쉬움
		•	참고: x86은 1~17바이트 명령어 (그래서 한 사이클은 어렵긴 하다)
	•	명령어 형식이 적고 규칙적임
		•	디코드와 레지스터 읽기를 한 번에 수행 가능
	•	Load/Store 주소 지정 방식
		•	3단계에서 주소 계산, 4단계에서 메모리 접근
	•	메모리 피연산자의 정렬 보장
		•	메모리 접근이 한 사이클이면 충분함
## Hazards
•	다음 명령어를 다음 사이클에 시작하지 못하게 하는 상황들
•	Structural Hazard
	•	필요한 자원이 사용 중일 때
•	data Hazard
	•	이전 명령어의 데이터 읽기/쓰기가 끝날 때까지 기다려야 함
•	Control Hazard
	•	제어 흐름 결정이 이전 명령어에 의존할 때
## Structural Hazard
•	자원 사용 충돌
•	MIPS 파이프라인에서 메모리가 하나일 경우
	•	Load/Store 명령은 데이터 접근 필요
	•	명령어 가져오기(fetch)는 해당 사이클 동안 **stall**해야 함
		•	결과적으로 파이프라인에 “**bubble**” 발생
	•	따라서 파이프라인 데이터 경로에는
		•	명령어/데이터 메모리를 분리하거나
		•	명령어/데이터 캐시를 별도로 구성해야 함
## 데이터 Hazard
- 명령어가 이전 명령어의 데이터 접근 완료에 의존할 수 있음
![[Pasted image 20250501152508.png|500]]
`add $s0, $t0, $t1`에서의 EX에서 연산 결과가 만들어지는데, 이로 인해 Forwarding 방식을 사용할 수 있다.
`add $s0, $t0, $t1`에서의 MEM은 사용하지는 않지만, 모든 stage를 균등하게 하기 위해 포함된다 (이렇게 하지 않으면 structure hazard가 발생할 수 있기 때문)
## Forwarding (Bypassing)
![[Pasted image 20250501152932.png|300]]
•	결과가 계산되자마자 사용
	•	레지스터에 저장되기를 기다리지 않음
	•	데이터 경로에 추가 연결이 필요함
## Load-Use data hazard
![[Pasted image 20250501153007.png|300]]
•	포워딩만으로 Stall을 항상 피할 수는 없음
	•	필요한 시점에 값이 계산되지 않았다면
	•	시간을 거슬러 포워딩할 수는 없음!
## Stall을 피하기 위한 코드 스케줄링
•	Load 명령의 결과를 바로 다음 명령에서 사용하지 않도록 코드 재배치
•	예: `A = B + E; C = B + F;` 와 같은 C 코드
![[Pasted image 20250501153038.png|300]]
## Control Hazards
•	분기 명령어는 제어 흐름을 결정함
	•	다음 명령어의 fetch는 분기 결과에 따라 달라짐
	•	파이프라인은 항상 올바른 명령어를 fetch할 수 없음
		•	분기의 ID 단계가 아직 처리 중일 수도 있음
•	MIPS 파이프라인에서는
	•	파이프라인 초기에 레지스터 비교 및 분기 대상 주소 계산 필요
	•	ID 단계에서 이를 수행하기 위한 하드웨어 추가
## Stall on Branch
•	분기 결과가 결정될 때까지 다음 명령어 fetch를 대기
![[Pasted image 20250501153148.png|400]]
## Branch Prediction
•	파이프라인이 길어질수록 분기 결과를 초기에 알기 어려움
	•	스톨 패널티가 치명적이 됨
•	분기 결과를 예측
	•	예측이 틀릴 경우에만 stall
•	MIPS 파이프라인에서는
	•	분기가 발생하지 않을 것으로 예측 가능
	•	분기 다음 명령어를 지연 없이 fetch
## MIPS with Predict Not Taken
![[Pasted image 20250501153255.png|400]]
## 현실적인 Branch 예측
•	정적 분기 예측
	•	일반적인 분기 동작에 기반
	•	예: 루프나 if문
		•	뒤로 가는 분기는 taken으로 예측
		•	앞으로 가는 분기는 not taken으로 예측
		=> for문은 이전으로 올라간다. 이로 인해 for loop은 branch할 확률이 높다는 가정을 한다.
•	동적 분기 예측
	•	하드웨어가 실제 분기 동작을 측정
		•	예: 각 분기의 최근 기록 저장 (하드웨어 수준에서 구현한다)
	•	미래 동작이 현재 경향을 계속 따른다고 가정
		•	예측이 틀리면 다시 fetch하고 기록 갱신

## 파이프라인 요약
•	파이프라이닝은 명령어 처리량 증가로 성능 향상
	•	여러 명령어를 병렬로 실행
	•	각 명령어의 지연 시간은 동일
•	다음과 같은 해저드 발생 가능
	•	구조적 해저드
	•	데이터 해저드
	•	제어 해저드
•	명령어 집합 구조가 파이프라인 구현의 복잡성에 영향을 줌
## Pipeline Registers
stage들 사이에 레지스터(rising edge가 발생할 때만 동작하는 register)가 필요하다
- 이전 사이클 안에서 생성된 정보를 가지고 있기 위해서
![[Pasted image 20250508154122.png|400]]
## Pipeline Operation
•	파이프라인된 데이터 경로를 통한 명령어의 사이클별 흐름
	•	“단일 클럭 사이클” 파이프라인 다이어그램
		•	한 사이클 내의 파이프라인 사용 현황을 보여줌
		•	사용된 자원을 강조
	•	참고: “다중 클럭 사이클” 다이어그램
		•	시간에 따른 동작 그래프
## IF for Load/Store
![[Pasted image 20250508154416.png|500]]
## ID for Load/Store
![[Pasted image 20250508154445.png|500]]
## EX for Load
![[Pasted image 20250508154508.png|500]]
## MEM for Load
![[Pasted image 20250508154533.png|500]]
## WB for Load
![[Pasted image 20250508154613.png|500]]
## EX for Store
![[Pasted image 20250508154645.png|500]]
## MEM for Store
![[Pasted image 20250508154706.png|500]]
## WB for Store
암것도 안함
## Multi-Cycle Pipeline Diagram
![[Pasted image 20250508155239.png|500]]
lw가 $13, 24($10) 이면, Reg에서 $10에서 읽기/쓰기가 동시에 일어난다.
하지만 이 것도 hazard가 발생하지 않고 문제가 발생하지 않는다 => 쓴 값이 바로 읽힌다.
## Pipelined Control
RegWrite같은 경우 ID stage에 있지만 WB stage에서 사용된다. 따라서 Control Signal도 넘겨줘야 한다
Control Signal 생성 시점: instruction decode 단계
![[Pasted image 20250508155545.png|300]]
![[Pasted image 20250508155605.png|500]]
RegDst -> 어느 reg에 쓸지를 결정함
ex) 
lw rt 숫자(rs) => rt에 씀
add rd, rs, rt => rd에다가 씀
## Data Hazards in ALU Instructions
sub $2, $1,$3
and $12,$2,$5
or $13,$6,$2
add $14,$2,$2
sw $15,100($2)
## Dependencies & Forwarding
![[Pasted image 20250508155912.png|500]]
이전 instruction & reg 번호와 conflict가 일어나는지 보고 forwarding 값을 쓸지 말지 