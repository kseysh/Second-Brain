[[The Processor - 중간]]
## ALU Control
Load/Store: F = add
Branch: F = subtract
R-type: F depends on funct field

opcode에서 파생된 2비트 ALUOp을 가정합니다 
Combinational logic derives ALU control
![[Pasted image 20250429231457.png|400]]
ALU control을 결정하기 위해 Truth table을 이용한다.
## The Main Control Unit
Control signals derived from instruction
![[Pasted image 20250501150833.png|500]]
R rd rs rt
lw rt N(rs)
sw rt N(rs)
beq rs rt N
## Datapath with Control
![[Pasted image 20250501150938.png|500]]

## R-Type Instruction
![[Pasted image 20250501151055.png|500]]
## Load Instruction
![[Pasted image 20250501151112.png|500]]
rt에 써야 하므로 RegDst는 rt를 읽은 0에다가 write해야하므로 RegDst가 0이다.
## Store Instruction
위에서 부터
X
1
X
0
0
1
0
## Branch on Equal Instruction
![[Pasted image 20250501151134.png|500]]
RegDst와 MemtoReg는 RegWrite이 0이므로 상관없음
## Datapath with Jumps Added
target PC = Top 4 bits of old PC +26-bit jump address + 00
PC+4의 위 4개 bit
가져온 값을 위로 두 칸 올려서 00을 맨 뒤로 만듬 (4를 더해줘야 하므로)
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
	•	*지연 시간(각 명령어의 실행 시간)은 감소하지 않음*
## 파이프라이닝과 ISA 설계
•	MIPS ISA는 파이프라이닝을 고려하여 설계됨
	•	모든 명령어는 32비트
		•	한 사이클 내에서 가져오고 디코딩하기 쉬움
		•	참고: x86은 1~17바이트 명령어 (그래서 한 사이클은 어렵긴 하다)
	•	명령어 형식이 적고 규칙적임
		•	decode와 레지스터 읽기를 한 번에 수행 가능
	•	Load/Store 주소 지정 방식
		•	3단계에서 주소 계산, 4단계에서 메모리 접근
	•	메모리 피연산자의 정렬 보장
		•	메모리 접근이 한 사이클이면 충분함
## Hazards
•	다음 명령어를 다음 사이클에 시작하지 못하게 하는 상황들
•	Structural Hazard
	•	필요한 자원이 사용 중일 때 (MIPS에서는 발생 X)
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
![[Pasted image 20250501152932.png|400]]
•	결과가 계산되자마자 사용
	•	레지스터에 저장되기를 기다리지 않음
	•	데이터 경로에 추가 연결이 필요함
## Load-Use data hazard
![[Pasted image 20250501153007.png|400]]
•	*포워딩만으로 Stall을 항상 피할 수는 없음*
	•	필요한 시점에 값이 계산되지 않았다면
	•	시간을 거슬러 포워딩할 수는 없음!
## Stall을 피하기 위한 코드 스케줄링
•	Load 명령의 결과를 바로 다음 명령에서 사용하지 않도록 코드 재배치
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
MIPs는 성능 최적화를 위해 비교연산을 ID 단계에서 전용 비교기를 통해 수행함 
![[IMG_B71D7C5BC464-1.jpeg|150]]
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
![[IMG_6B55A7C0898C-1.jpeg|500]]
## WB for Load
![[IMG_62942C41B2DC-1.jpeg|500]]
Load 명령어를 사용할 때, WB단계에서 Register 메모리에 Write register에 있는 값을 저장하게 되는데, 위 그림처럼 수행하면 ID 단계에서 읽었던 잘못된 write register 값을 가져오는 문제가 발생할 수 있다. 따라서 write register는 아래와 같이 저장되어야 한다.
### Corrected Data path for Load
![[IMG_12CF0663B16C-1.jpeg|500]]
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
위 예제에서 `add $14,$2,$2` 와 `sw $15,100($2)`는 data hazard가 발생하지 않는다.
`and $12,$2,$5` 와 `or $13,$6,$2`는 data hazard가 발생하는데, 결과물 값을 확인해보면 sub의 결과물을 reg에 저장하는 것은 CC5지만, 결과물 자체는 CC 3이 끝나고 생긴다. 
and 에서 data hazard가 발생하는 reg가 필요한 시점은 CC4이므로 forwarding을 이용해 data hazard를 해결한다.

or에서도 Reg에서 값을 가져오는 것은 CC4이고 sub에서 결과물을 레지스터에 저장하는 것은 CC5지만, forwarding을 통해 Reg에 저장하는 결과물이 나오는 시점인 CC3, or에서 실제 결과물이 필요한 시점인 CC5로 forwarding을 진행해 data hazard를 해결한다.

이전 instruction & reg 번호와 conflict가 일어나는지 보고 forwarding 값을 쓸지 말지 봐야한다.
다음 / 다다음 instruction까지만 신경쓰면 된다.
## Detecting the Need to Forward
•	파이프라인을 따라 레지스터 번호(register number) 를 전달한다.
	•	예시: ID/EX.RegisterRs는 ID/EX 파이프라인 레지스터에 저장된 RS 소스 레지스터의 번호를 의미한다.
•	EX(실행) 단계에서의 ALU 피연산자 레지스터 번호는 다음과 같이 주어진다:
	ID/EX.RegisterRs, ID/EX.RegisterRt
•	데이터 해저드(data hazard) 는 다음과 같은 경우에 발생한다:
	![[Pasted image 20250508161007.png|400]]
	→ EX 단계 바로 다음인 MEM 단계에서 쓰려는 레지스터가, 현재 EX 단계에서 ALU 입력으로 사용되는 소스 레지스터와 같을 때
	![[Pasted image 20250508161027.png|400]]
	→ MEM 다음인 WB 단계에서 쓰려는 레지스터가, 현재 EX 단계에서 사용하는 소스 레지스터와 같을 때
![[Pasted image 20250508160827.png|300]]
후속 명령어가 이전 명령어의 결과를 필요로 하지만 그 결과가 아직 레지스터에 쓰이지 않은 상태일 때, 데이터 해저드가 발생한다

•	하지만 오직 forwarding 명령어가 레지스터에 값을 쓸 경우에만 해당됩니다!
	•	EX/MEM.RegWrite, MEM/WB.RegWrite
•	그리고 해당 명령어의 목적지 레지스터가 $zero가 아닐 경우에만 해당됩니다
	•	EX/MEM.RegisterRd ≠ 0, MEM/WB.RegisterRd ≠ 0
![[Pasted image 20250515171540.png|200]]

따라서, 아래 조건을 확인해야 함
1. 이전 instruction이 R-format인가?
2. 이전 instruction이 RegWrite를 하는가?
3. 이전 instruction의 destination(rd)가 0인가?
## Forwarding Paths
![[Pasted image 20250515173009.png|500]]
and $12, *$2*, $5
sub *$2*, $1, $3
$2가 겹치는 상황에서의 forwarding
## Forwarding Conditions
굳이 물어보지는 않음.. 읽어만보자

| Mux 제어        | 소스     | 설명                                             |
| ------------- | ------ | ---------------------------------------------- |
| ForwardA = 00 | ID/EX  | 첫 번째 ALU 피연산자는 레지스터 파일에서 가져옵니다.                |
| ForwardA = 10 | EX/MEM | 첫 번째 ALU 피연산자는 이전 ALU 결과에서 포워딩됩니다.             |
| ForwardA = 01 | MEM/WB | 첫 번째 ALU 피연산자는 데이터 메모리나 더 이전의 ALU 결과에서 포워딩됩니다. |
| ForwardB = 00 | ID/EX  | 두 번째 ALU 피연산자는 레지스터 파일에서 가져옵니다.                |
| ForwardB = 10 | EX/MEM | 두 번째 ALU 피연산자는 이전 ALU 결과에서 포워딩됩니다.             |
| ForwardB = 01 | MEM/WB | 두 번째 ALU 피연산자는 데이터 메모리나 더 이전의 ALU 결과에서 포워딩됩니다. |
## Double Data Hazard
•	다음 명령어 순서를 고려해 봅시다:
1. add *$1*,$1,$2 
2. add *$1*,*$1*,$3  
3. add $1,*$1*,$4  
2,3 => EX/MEM 단계의 값을 forwarding
1, (2,3) => MEM/WB 단계의 값을 forwarding
	1. register 번호 체크
	2. EX/MEM forwarding이 없을 경우에만
=> 둘 다 필요하면, 가장 최근 값만 forwarding 해준다.

•	두 종류의 데이터 해저드가 발생합니다
	•	가장 최근 값을 사용해야 합니다
•	MEM 해저드 조건을 수정해야 합니다
	•	EX 해저드 조건이 참이 아닐 경우에만 forwarding 하도록 해야 합니다
## 수정된 Forwarding 조건
![[Pasted image 20250515173721.png|500]]
## Datapath with Forwarding
회로는 중요하지 않음, 흐름만 보면 된다.
![[Pasted image 20250520154245.png|500]]
## Load-Use Data Hazard
![[Pasted image 20250520154357.png|500]]

## Load-Use Data Hazard
•	사용하는 명령어가 ID 단계에서 디코드될 때 확인
•	ID 단계에서 ALU 피연산자 레지스터 번호는 다음과 같습니다:
	•	`IF/ID.RegisterRs, IF/ID.RegisterRt`
•	Load-use 해저드는 다음 조건에서 발생합니다:
	`ID/EX.MemRead and  `
		`((ID/EX.RegisterRt = IF/ID.RegisterRs) or  `
		 `(ID/EX.RegisterRt = IF/ID.RegisterRt))`
•	감지되면 stall을 발생시키고 bubble을 삽입합니다
## 파이프라인을 멈추는 방법
•	ID/EX 레지스터의 제어 값을 0으로 설정
	•	EX, MEM, WB 단계는 nop(무연산)을 수행
•	PC와 IF/ID 레지스터의 업데이트를 방지
	•	사용하는 명령어는 다시 디코드되고
	•	다음 명령어는 다시 fetch됩니다
	•	1 사이클의 stall로 MEM이 lw 명령어의 데이터를 읽을 수 있도록 함
		•	이후 EX 단계로 forwarding이 가능함
![[Pasted image 20250520154500.png|400]]	
## Load-Use Data Hazard
![[Pasted image 20250520154600.png|500]]
![[Pasted image 20250520154728.png|500]]
## Datapath with Hazard Detection
![[Pasted image 20250520154837.png|500]]
## Stalls and Performance
•	Stall은 성능을 떨어뜨립니다
	•	하지만 정확한 결과를 위해 필요합니다
•	컴파일러는 해저드와 stall을 피하도록 코드를 재배치할 수 있습니다
	•	이를 위해 파이프라인 구조에 대한 지식이 필요합니다
## Branch Hazards
만약 branch 결과가 MEM에서 결정된다면,
![[Pasted image 20250520154955.png|500]]
branch not taken 예측 후 틀리면 branch 하는 과정
## 분기 지연 줄이기
•	분기 결과를 ID 단계에서 판단하도록 하드웨어 이동
	•	타겟 주소 덧셈기
	•	레지스터 비교기
#### example - branch taken
```
36: sub $10, $4, $8  
40: beq $1, $3, 7  
44: and $12, $2, $5  
48: or $13, $2, $6  
52: add $14, $4, $2  
56: slt $15, $6, $7  
...  
72: lw $4, 50($7)  
```

![[Pasted image 20250520155226.png|500]]
요약: 원래는 비교를 EX stage에서 했는데, ID 과정으로 옮겼다 정도만 기억
![[Pasted image 20250520155239.png|500]]
## 분기에 대한 데이터 해저드
•	비교에 사용되는 레지스터가 앞의 두 번째 또는 세 번째 ALU 명령어의 목적지일 경우
![[Pasted image 20250520155528.png|400]]
•	forwarding으로 해결 가능
•	비교 레지스터가 앞의 ALU 명령어 또는 두 번째 앞의 load 명령어의 목적지일 경우
	•	1 사이클 stall 필요
![[Pasted image 20250520155639.png|500]]
beq에서는 표시된 1, 4가 필요한데. forwarding으로 땡기기는 무리이다.
	
•	비교 레지스터가 바로 앞의 load 명령어의 목적지일 경우
	•	2 사이클 stall 필요
![[Pasted image 20250520155743.png|500]]
## 동적 분기 예측
•	더 깊은 파이프라인과 슈퍼스칼라 구조에서는 분기 패널티가 커짐
•	동적 예측 사용
	•	분기 예측 버퍼 (branch history table) 사용
	•	최근 분기 명령어 주소로 인덱싱
	•	결과(taken/not taken)를 저장
	•	분기 실행 시
		•	테이블 확인 후 동일한 결과를 예상
		•	fall-through 또는 타겟으로 fetch 시작
		•	예측이 틀리면 파이프라인을 flush하고 예측을 반전
if, 0x1000400 beq가 taken이었다면, 400은 taken이었다고 적어놓는다 (다 사용하면 1 byte를 전부 차지하기 때문)
## 1비트 예측기의 한계
•	내부 루프 분기에서 두 번 틀림
![[Pasted image 20250520155908.png|400]]
•	마지막 루프에서 taken으로 잘못 예측
•	다음 루프의 첫 번째 반복에서 not taken으로 잘못 예측
## 2비트 예측기
•	두 번 연속 오예측일 때만 예측 변경
![[Pasted image 20250520155933.png|500]]
## 2비트 예측기 (다른 버전)
•	두 번 연속 틀릴 때만 예측 변경
![[Pasted image 20250520160050.png|500]]
이 버전은 두 번 틀렸을 때 강한 Not Taken으로 바뀜.
다른 버전은 3번이었음
## 분기 타겟 주소 계산
•	예측기를 써도 타겟 주소 계산은 필요
	•	분기 발생 시 1 사이클 패널티
•	분기 타겟 버퍼
	•	타겟 주소 캐시
	•	명령어 fetch 시 PC로 인덱싱
		•	hit이고 분기 예측이 taken이면 즉시 타겟 fetch 가능
![[Pasted image 20250520160120.png|500]]
## 명령어 수준 병렬성 (ILP)
•	파이프라이닝: 여러 명령어를 병렬로 실행
•	ILP를 증가시키기 위한 방법: -> system 처리량 ↑
	•	더 깊은 파이프라인
		•	각 단계 작업을 줄여 클록 사이클 단축
	•	Multiple Issue (여러 instruction을 동시에 발행)
		•	파이프라인 단계를 복제해 여러 개의 파이프라인 구성
		•	클록 사이클마다 여러 명령어를 시작
		•	CPI < 1 이므로 IPC 사용 (CPI<sup>-1</sup> = IPC)
		•	예시: 4GHz, 4-way issue
			•	16 BIPS, 최대 CPI = 0.25, 최대 IPC = 4
		•	하지만 실제론 의존성으로 인해 감소
## Multiple Issue
•	Static multiple issue (PC와 PC+4를 항상 같이 fetch)
	•	컴파일러가 함께 발행될 명령어를 그룹화 (sw가 수행)
	•	이를 “issue slot”에 패키징
	•	해저드를 감지하고 피함
•	Dynamic multiple issue (CPU가 instruction을 여러개 미리 fetch -> 그 중 상관없는 것들을 모아서 같이 issue)
	•	CPU가 명령어 스트림을 분석해 각 사이클마다 발행할 명령어 선택
	•	컴파일러는 명령어 순서를 재배열해 도움 줄 수 있음
	•	CPU는 런타임에 고급 기술로 해저드 해결
## Speculation (추정)
•	명령어에 대해 “예측”하여 실행
	•	가능한 한 빨리 작업 시작
	•	예측이 맞으면 그대로 실행 완료
		•	틀리면 롤백하고 올바른 실행 수행
		•	정적 및 동적 다중 발행에서 공통적으로 사용
•	예시:
	•	분기 결과 예측 후 롤백
	•	load 명령어를 예측하고, 위치가 변경되면 롤백
```
sw $t0, 100($t0)
...
lw $t1, 100($s1)
```
상관이 없어보이지만, $s0 = $s1 이고, 이 순서가 바뀌면 문제가 발생함
=> 상관없다고 추정한 것이 실패한 것
## Compiler/Hardware Speculation
•	컴파일러는 명령어 순서 재배열 가능
	•	예: load를 branch보다 먼저
	•	잘못된 추측을 복구하는 “fix-up” 명령어 포함 가능 (Compiler가 recover code 추가)
•	하드웨어는 실행 가능한 명령어를 미리 탐색
	•	실제로 필요할 때까지 결과를 버퍼에 보관
	•	잘못된 추측이 감지되면 버퍼 flush
## 추측 실행과 예외 처리
•	추측 실행된 명령어에서 예외가 발생하면 어떻게 하나?
	•	예: null 포인터 체크 이전에 load 실행
•	정적 추측 실행
	•	예외를 지연시키는 ISA 지원 추가 가능
•	동적 추측 실행
	•	명령어가 완료될 때까지 예외를 버퍼링 가능 (완료되지 않을 수도 있음)
## Static Multiple Issue
•	컴파일러는 명령어들을 “issue packet”으로 묶음
	•	하나의 사이클에서 발행할 수 있는 명령어 그룹
	•	파이프라인 자원 요구에 따라 결정됨
•	issue packet은 매우 긴 명령어로 생각할 수 있음
	•	여러 동시 연산을 지정
	•	→ 매우 긴 명령어 워드(VLIW: Very Long Instruction Word)
## Scheduling Static Multiple Issue
•	컴파일러는 일부 또는 모든 해저드를 제거해야 함
	•	명령어를 issue packet으로 재배열
	•	하나의 packet 안에는 의존성이 없어야 함
	•	packet 간에는 의존성이 있을 수 있음
		•	ISA마다 다르며, 컴파일러가 이를 알아야 함
	•	필요 시 nop(no operation)으로 채움

## MIPS with Static Dual Issue
•	두 개의 명령어로 구성된 issue packet
	•	하나는 ALU/branch 명령어
	•	하나는 load/store 명령어
	•	64비트 정렬 -> PC가 8씩 증가함
		•	ALU/branch 명령어 먼저, 그다음 load/store
		•	사용되지 않는 명령어는 nop으로 채움
![[Pasted image 20250520121054.png|400]]
![[Pasted image 20250520121416.png|500]]
if,
`add $s0, $s1, $s2`
`sw $t0, 100($t1)`
register로 들어가는 input port가 두개로 늘어난 것을 볼 수 있음 (두 배의 읽기를 수행해야 하기 때문)
쓰기도 두 배로 수행해야하기 때문에 output port가 두 개로 늘어남
ALU도 하나 더 늘어남 (하나는 ALU, 하나는 lw/sw용)
R-format은 MEM skip, sw는 Data memory를 지나감
data memory는 그대로
## Hazards in the Dual-Issue MIPS
•	더 많은 명령어가 병렬로 실행됨
•	EX 데이터 해저드
	•	단일 발행에서는 forwarding으로 stall을 피함
	•	이제는 동일 packet 안에서 ALU 결과를 load/store에서 사용할 수 없음
		`add $t0, $s0, $s1  `
		`load $s2, 0($t0)`
	•	두 개의 packet으로 분리해야 하며, 사실상 stall 발생
•	Load-use 해저드
	•	여전히 1사이클 사용 지연 (nop으로 채워보낸다면)
	•	하지만 이제 두 개의 명령어가 관련됨
•	더 공격적인 스케줄링이 요구됨
#### 스케줄링 예시
•	다음 루프를 dual-issue MIPS에 맞게 스케줄링

```
Loop: lw $t0, 0($s1)      # $t0 = 배열 요소  
      addu $t0, $t0, $s2  # 스칼라 값 더하기  
      sw $t0, 0($s1)      # 결과 저장  
      addi $s1, $s1, -4   # 포인터 감소  
      bne $s1, $zero, Loop # 분기 조건 검사  

while(p!=0){
	*p += a;
	p--;
	// $s1 = p
	// $s2 = a
}
```
![[Pasted image 20250520122226.png|400]]
addu -> lw에서 불러온 s1이 addu때 사용되므로 lw와 떨어져야 함
addi와 bne도 한 사이클 떨어져 있어야 함
addi는 1-1로 가도 되고, 2-1에 있어도 됨
addu가 되어야 sw가 되기 때문에 sw의 자리가 저렇게 됨

•	IPC = 5/4 = 1.25 (참고: 최대 IPC = 2)
## Loop Unrolling
Loop Unrolling의 두 가지 효과
	•	루프 본문을 복제하여 더 많은 병렬성 노출 (loop 내부의 크기를 늘려 병렬성을 크게 한다.)
	•	루프 제어 오버헤드 감소 (beq instruction 비중을 줄인다)
•	복제마다 다른 레지스터 사용
	•	이를 “레지스터 이름 바꾸기(register renaming)”라고 함
	•	루프에 의해 전달되는 “anti-dependency” 회피 (같은 레지스터를 사용하지 않으므로)
		•	같은 레지스터를 store하고 다시 load하는 경우
		•	“이름 의존성(name dependence)”이라고도 함
			•	같은 레지스터 이름을 재사용
#### example
```
while(p!=0){
	*p += a;
	*(p-1) += a;
	*(p-2) += a;
	*(p-3) += a;
	p = p-4
	// $s1 = p
	// $s2 = a
}
```
branch에서 하는 일이 많을 수록 branch를 위한 instruction의 비중이 줄어들어 내부적으로 parallelism을 사용할 확률이 올라간다
![[Pasted image 20250520123711.png|400]]
IPC = 14/8 = 1.75
- 하지만, register 활용률이 증가해버리고, code size가 커진다.
- loop size에 따른 조건을 더 철저하게 확인해야 한다.
## Dynamic Multiple Issue
여기부터는 개념적으로 받아들이자
•	"Superscalar" 프로세서 (CPU HW)
•	CPU가 매 사이클마다 0, 1, 2, … 개의 명령어를 발행할지 결정
	•	structural and data hazards 회피
•	컴파일러의 스케줄링 필요 없음
	•	하지만 여전히 도움이 될 수 있음
	•	코드 의미는 CPU가 보장
## Dynamic Pipeline Scheduling
•	CPU가 명령어를 out-of-order로 실행해 stall을 피할 수 있도록 허용
	•	결과는 반드시 순서대로 레지스터에 기록
•	예시:
```
lw $t0, 20($s2)  
addu $t1, $t0, $t2  
sub $s4, $s4, $t3  
slti $t5, $s4, 20  
```
•	addu가 lw를 기다리는 동안 sub를 먼저 시작 가능
앞에서 한 것을 하드웨어가 판단하는 것
이런 것을 하기 위해서 CPU는 Instruction을 미리 여러개 읽어놓아야 한다.
## Dynamically Scheduled CPU
![[Pasted image 20250520124953.png|500]]
Reservation Station이 추가되고, 얘가 re-order instructions
주변 reservation station을 보면서 순서가 재배치 된다.
실행은 먼저하지만, 결과 적용은 순서대로 된다 이를 위해 결과가 Commit Unit에 저장해놨다가 순서대로 commit을 진행한다.
## Register Renaming
•	예약 스테이션(reservation station)과 reorder buffer는 register renaming를 효과적으로 수행
•	명령어가 예약 스테이션에 발행될 때:
	•	피연산자가 레지스터 파일이나 reorder buffer에 존재하면
		•	reservation station으로 복사
		•	더 이상 레지스터에 필요 없으므로 덮어쓰기 가능
	•	피연산자가 아직 준비되지 않았으면
		•	function unit이 나중에 reservation station에 제공
		•	레지스터 업데이트가 필요하지 않을 수도 있음
## Speculation (추측 실행)
•	분기를 예측하고 명령어 발행 계속
	•	분기 결과가 결정되기 전까지 커밋하지 않음
•	Load Speculation
	•	load 및 캐시 miss 지연 회피
		•	유효 주소 예측
		•	로드 값 예측
		•	완료되지 않은 store보다 먼저 load 수행
		•	store된 값을 load 유닛에 우회 전달
	•	추측이 해결될 때까지 load는 커밋하지 않음

```
sw $s0, 10($s1)
lw $t0, 10($t2)
```
만약 $t2 == $t1이면, dependency가 있었던 것
## 왜 동적 스케줄링을 하는가?
•	그냥 컴파일러가 스케줄링하도록 두면 안 되는가?
•	모든 stall이 예측 가능한 것은 아님
	•	예: 캐시 miss
•	항상 분기를 피해서 스케줄링할 수 없음
	•	분기 결과는 런타임에 결정됨
•	동일한 ISA라도 구현에 따라 지연과 해저드가 다름
## Multiple Issue이 효과적인가?
•	효과는 있지만 기대만큼은 아님
•	프로그램에는 실제 의존성이 존재
•	제거하기 어려운 의존성 존재
	•	예: 포인터 aliasing
•	노출되기 어려운 병렬성 존재
	•	명령어 발행 시 창(window) 크기 제한
•	메모리 지연 및 대역폭 제한
	•	파이프라인을 가득 유지하기 어려움
•	잘 수행되면 추측 실행이 도움이 될 수 있음
성능을 높이려면 ILP를 증가
- pipelining
- parallelism
## 전력 효율성
•	동적 스케줄링과 추측 실행의 복잡도는 전력을 요구
•	더 단순한 여러 코어가 더 나은 선택일 수 있음
![[Pasted image 20250520130811.png|400]]
추세만 보자
처음: pipeline도 적고, speculation도 안한다
점차 pipeline stage를 올리고 speculation도 한다
어느순간 pipeline을 14로 고정하고 issue width도 4로 고정한다, 대신, cpu core가 늘어남
## cortex A53 and Intel i7
![[Pasted image 20250522120732.png|400]]
## ARM Cortex-A53 Pipeline
![[Pasted image 20250522121109.png|400]]
## ARM Coretex-A53 Branch Prediction
![[Pasted image 20250522121318.png|500]]
mis prediction rate -> 그로 인해 버려지는 일의 양
## ARM Cortex-A53 Performance
![[Pasted image 20250522121530.png|300]]
## Core i7 Pipeline
눈여겨볼 필요는 없음.. 이런게 있다.
![[Pasted image 20250522121720.png|300]]
CISC지만, macro op -> micro op로 나눠서 pipeline 설계에 적합한 형태로 바꿔 clock cycle time을 줄인다.
## Core i7 Performance
![[Pasted image 20250522122155.png|400]]
한 사이클에 4개씩 보낼 수 있으니, Ideal CPI는 0.25
얘네는 시험에 안나옴..
## Matrix Multiply
![[Pasted image 20250522122401.png|400]]
j와 k를 풀어서 썼는데, 이렇게 하면 루프 한 번에 16번의 연산을 실행함
## Performance Impact
![[Pasted image 20250522122515.png|300]]
GFLOPS: Giga Floating Point Operations Per Second (성능이라 생각하자)
## 오해들 (Fallacies)
•	파이프라이닝은 쉽다(!)
	•	기본 개념은 쉬움
	•	진짜 문제는 세부사항에 있음
		•	예: 데이터 해저드 감지
•	파이프라이닝은 기술과 무관하다
	•	그럼 왜 항상 파이프라이닝을 하지 않았는가?
	•	트랜지스터 수가 증가하면서 더 발전된 기법 가능
	•	파이프라인 관련 ISA 설계는 기술 발전을 고려해야 함
		•	예: 조건 실행 명령어(predicated instruction)
## 주의점들 (Pitfalls)
•	좋지 않은 ISA 설계는 파이프라이닝을 어렵게 만듦
	•	예: 복잡한 명령어 집합 (VAX, IA-32)
		•	파이프라이닝 구현을 위해 상당한 오버헤드 필요
		•	IA-32는 마이크로 명령어 방식 사용
•	예: 복잡한 주소 지정 방식
	•	레지스터 업데이트 부작용, 메모리 간접 참조
•	예: 지연된 분기
	•	고급 파이프라인은 긴 지연 슬롯을 가짐
## 결론
•	ISA는 데이터패스 및 제어 설계에 영향을 미침
•	데이터패스 및 제어는 ISA 설계에 영향을 줌
•	파이프라이닝은 병렬성을 활용하여 명령어 처리량을 향상
	•	초당 더 많은 명령어 수행
	•	각 명령어의 지연시간(latency)은 줄지 않음
•	해저드 종류: 구조적, 데이터, 제어
•	다중 발행 및 동적 스케줄링 (ILP)
	•	의존성은 병렬성에 한계를 줌
	•	복잡성은 전력 장벽(power wall)을 유발함

