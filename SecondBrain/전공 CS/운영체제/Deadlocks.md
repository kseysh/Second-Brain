## 시스템 모델
•	시스템은 자원들로 구성됨
•	자원 유형: R1, R2, …, Rm
	•	예: CPU 사이클, 메모리 공간, I/O 장치
•	각 자원 유형 Ri는 Wi 개의 인스턴스를 가짐
•	각 프로세스는 자원을 다음과 같은 방식으로 사용함
	•	요청
	•	사용
	•	반환
## 교착 상태 (Deadlock)
•	교착 상태의 정의
	•	하나의 프로세스 집합 내에서 각 프로세스가 다른 프로세스가 가진 어떤 것을 기다리고 있는 상황
	•	모두가 기다리고 있으므로, 아무도 기다리는 자원을 제공할 수 없음
•	세마포어를 사용한 교착 상태 예시
![[Pasted image 20250508173346.png|300]]
## 교착 상태의 특성
•	네 가지 조건이 동시에 성립될 때 교착 상태가 발생할 수 있음:
	•	상호 배제 (Mutual exclusion)
		•	한 번에 하나의 프로세스만 자원을 사용할 수 있음
	•	비선점 (No preemption)
		•	한 번 할당된 자원은 강제로 빼앗을 수 없음
	•	보유와 대기 (Hold and wait)
		•	하나 이상의 자원을 보유한 프로세스가 다른 자원을 요청하며 대기 중
	•	순환 대기 (Circular wait)
		•	자원을 가진 프로세스들이 서로가 가진 자원을 기다리는 원형의 대기 관계 존재
## Deadlock: Resource-Allocation Graph
![[Pasted image 20250508173439.png|150]]
![[Pasted image 20250508173742.png|300]]
순환고리가 만들어졌다고 무조건 데드락이 걸리는 것은 아님
## Deadlock Handling
•	교착 상태 해결책은 두 가지 일반적인 범주로 나뉨:
	•	교착 상태 예방/회피
		•	시스템을 구성할 때 교착 상태가 절대 발생하지 않도록 설계
		•	자원 효율성이 낮아질 수 있음
	•	교착 상태 탐지 및 복구
		•	시스템이 교착 상태인지 판단하고, 심각한 조치를 취함
		•	하나 이상의 프로세스를 종료하여 자원을 회수해야 할 수 있음
## 교착 상태 예방
•	네 가지 조건 중 하나를 피하는 방식:
	•	*No mutual exclusion*
		•	공유 자원에 대한 배타적 접근을 허용하지 않음
		•	많은 애플리케이션에서 비현실적
	•	*Preemption*
		•	자원을 보유한 프로세스가 새로운 자원을 요청했을 때 즉시 할당할 수 없다면, 기존 자원 모두를 반납시킴
		•	반납된 자원은 대기 목록에 추가됨
		•	해당 프로세스는 모든 자원을 다시 얻을 수 있을 때까지 재시작되지 않음
	•	*No hold and wait*
		•	프로세스가 자원을 요청할 때, 다른 자원을 보유하고 있지 않아야 함을 보장해야 함
			•	예) 프로세스가 필요한 모든 자원을 한 번에 요청하게 만듦
		•	필요한 자원을 전부 다 얻거나, 아니면 전부 다 얻을 때까지 대기
		•	프로세스 입장에서는 불편함
			•	필요한 자원을 미리 예측하기 어려워, 자원을 매우 비효율적으로 사용할 수 있음
			•	낮은 자원 활용률 또는 기아 상태가 발생할 수 있음
	•	*No circular wait*
		•	자원 요청에 순서를 부여함
			•	예: 먼저 S 자원들, 그 다음 T 자원들을 요청
		•	모든 프로세스가 같은 순서를 따르도록 강제함
## Deadlock Example
![[Pasted image 20250513164530.png|300]]
## 교착 상태 회피
• 시스템이 *사전에 추가적인 정보*를 가지고 있어야 한다는 것을 전제로 함
	• 가장 단순하고 유용한 모델은 각 프로세스가 필요할 수 있는 자원 종류별 *최대 개수*를 미리 선언하는 방식임
	• 교착 상태 회피 알고리즘은 *자원 할당 상태*를 동적으로 검사하여 순환 대기(circular-wait) 조건이 발생하지 않도록 보장함
	• *자원 할당 상태*는 사용 가능한 자원의 수, *할당된 자원의 수*, 그리고 *프로세스들의 최대 요구량*으로 정의됨
## Banker’s Algorithm
은행이 100억이 있을 때, A,B,C가 각각 20,30,30억을 빌린다.
이후 A,B,C가 각각 40, 30, 40억을 추가적으로 빌리려고 한다면 deadlock이 발생하며 아무도 빌릴 수 없게 된다.

따라서 A,B,C는 각각 60억, 60억, 70억을 빌릴 수 있을 것 같다고 미리 말해야 한다.
A,B가 각각 20,30억을 빌려가면 50억이 남는데, 이 때 C에게 돈을 빌려줄지 말지 고민해야 한다.
빌려주게 되면, A,B,C가 추가적으로 돈을 빌리고자 할 때 deadlock이 발생하기 때문이다.

### 용어 정의
•	P = {P1, P2, …, Pn} : 시스템 내 모든 프로세스 집합 (n개의 process)
•	R = {R1, R2, …, Rm} : 시스템 내 모든 자원 유형 집합 (m가지의 resource)
•	*safe state*: 프로세스에 최대 자원을 할당해도 교착 상태가 발생하지 않는 순서가 존재하는 상태
	•	안전 상태에서는 반드시 *safe sequence* <P1, P2, …, Pn> 가 존재
		•	safe sequence: 모든 프로세스가 자원을 요청하고 실행을 마칠 수 있는 순서
		•	Pi가 요청할 수 있는 자원은 현재 사용 가능한 자원 + Pi보다 앞선 모든 프로세스가 반환할 자원으로 충족 가능해야 함
			•	P<sub>i</sub>의 자원 요구가 즉시 충족되지 않으면, P<sub>i</sub>는 앞선 P<sub>j</sub>들이 종료될 때까지 기다림
			•	P<sub>j</sub>가 종료되면, Pi는 필요한 자원을 얻고 실행 후 자원을 반환하고 종료
			•	P<sub>i</sub>가 종료되면 다음 P<sub>i+1</sub>이 자원을 얻어 실행함
•	unsafe state: 교착 상태로 이어질 수 있음
### 특징
•	시스템이 비안전 상태에 진입하지 않도록 보장해야 함
•	각 프로세스는 사전에 최대 자원 요구량을 선언해야 함
•	자원을 요청할 때 기다릴 수 있음
•	자원을 모두 할당받은 후에는 한정된 시간 내에 반환해야 함
### 기호 표기
•	`n` = 프로세스 수, m = 자원 유형 수
•	`Available[1:m]`: 각 자원 유형별 사용 가능한 수 (각 resource마다 남은 instance의 수)
•	`Max[1:n, 1:m]`: 각 프로세스의 자원 최대 요구량 
•	`Allocation[1:n, 1:m]`: 각 프로세스에 현재 할당된 자원 수
•	`Need[1:n, 1:m]`: 각 프로세스의 남은 자원 요구량
•	관계: `Max[i, j]` = `Allocation[i, j]` + `Need[i, j]`
### 알고리즘
•	안전성 알고리즘 (현재 safe state에 있는지 판단)
•	자원 요청 알고리즘 (어떤 resource request가 있을 때 이를 허가해도 되는지를 판단)
#### Safety Algorithm Example
5 Processes: P0, P1, P2, P3, P4
3 resources: A(10 instance), B(5 instances), and C (7 instances)

- Snapshot at time T0
![[Pasted image 20250513171139.png|400]]
Safe sequence: < P1, 

- Snapshot at time T1
![[Pasted image 20250513171332.png|400]]
Safe sequence: < P1, P3

- Snapshot at time T2
![[Pasted image 20250513171506.png|400]]
Safe sequence: < P1, P3, P4

- Snapshot at time T3
![[Pasted image 20250513171536.png|400]]
Safe sequence: < P1, P3, P4, P2

- Snapshot at time T4
![[Pasted image 20250513171606.png|400]]
Safe sequence: < P1, P3, P4, P2, P0> exists

## Safety Algorithm pseudo code
```
1. Initialize Work[1:m] and Finish[1:n]
	Work = Available // 
	Finish[i] = false for i = 1, 2, …, n
2. Find an i such that both
	(a) Finish[i] = false
	(b) Needᵢ <= Work
	If no such i exists, go to step 4
3. Work = Work + Allocationᵢ
	Finish[i] = true
	Go to step 2
4. If Finish[i] = true for all i, then the system is in a safe state
```
#### Resource Request Algorithm
5 Processes: P0, P1, P2, P3, P4
3 resources: A(10 instance), B(5 instances), and C (7 instances)

- Snapshot at time T1
![[Pasted image 20250513172125.png|400]]
New request from P0 (0,2,0)

- Snapshot at time T1
![[Pasted image 20250513172347.png|400]]
New request from P0 (0,2,0) -> Make unsafe state

#### Pseudo code
• Resource-Request Algorithm for Process Pᵢ
• Requesti = request for process Pᵢ
```
1. If Requestᵢ <= Needi, go to step 2
	Otherwise, raise error condition (exceed maximum claim)
2. If Requestᵢ <= Available, go to step 3
	Otherwise, Pᵢ must wait for the resource
3. Pretend to allocate requested resources to Pᵢ by modifying the state as follows
		Available = Available - Requestᵢ
		Allocationᵢ = Allocationᵢ + Requestᵢ
		Needᵢ = Needᵢ- Requestᵢ
	If safe -> the resources are allocated to Pᵢ
	If unsafe -> Pᵢ must wait, and the old resource-allocation state is restored
```

### Banker's Algorithm이 현실적이지 못한 이유
- Max를 알기 어렵다
- 자원 할당 wait이 길어질 수 있음
- 너무 보수적 -> resource utilization이 낮은 방법
### Example
![[Pasted image 20250513172841.png|300]]
- Is the system a  safe state?

- Can we immediately approve the request (0, 4, 2, 0) for P1?

## Deadlock Detection
•	교착 상태 처리 메커니즘의 한계
	•	교착 상태를 예방하는 것은 비용이 많이 들거나 비효율적임
	•	탐지도 비용이 많이 들고 복구는 거의 불가능함
	•	프로세스가 이상한 상태에 있을 경우는 어떻게 할 것인가?
		•	특히 차량과 같은 미션 크리티컬 시스템에서는 더욱 그러함

•	각 자원 타입이 단일 인스턴스인 경우 
	•	그래프를 그려보면 된다.
	•	사이클의 존재는 교착 상태의 필요충분 조건임
•	자원 타입에 여러 인스턴스가 있는 경우
	•	Banker's Algorithm과 유사한 교착 상태 탐지 알고리즘을 사용
### Deadlock Detection - Single Instance
•	각 자원 타입이 단일 인스턴스인 경우
	•	Wait-for 그래프를 유지
	•	주기적으로 그래프 내의 사이클을 탐색하는 알고리즘을 호출
		•	사이클이 존재하면 교착 상태가 존재
	•	그래프 내 사이클을 탐지하는 알고리즘의 시간 복잡도: O(n²), n = 정점 수
![[Pasted image 20250513174344.png|300]]
### Deadlock Detection - Multiple Instance
•	자원 타입에 여러 인스턴스가 있는 경우의 교착 상태 탐지 알고리즘 (example 풀 줄 알면 될 듯)
1.	`Work[1:m]`과 `Finish[1:n]` 초기화
	Work = Available
	Allocationᵢ ≠ 0이면 `Finish[i] = false`, 그렇지 않으면 true
2.	다음 조건을 만족하는 i를 찾음
	`Finish[i]` = false
	Requestᵢ <= Work
	만족하는 i가 없다면 4단계로 이동
3.	Work = Work + Allocationᵢ
	`Finish[i]` = true
	2단계로 이동
4.	`Finish[i] == false`인 i가 존재하면 시스템은 교착 상태이며 Pᵢ는 교착됨

Banker's Algorithm과 다른 점은 MAX값을 모른다는 것이 가장 큰 차이점
#### Multiple Instance Example
5 Processes: P0, P1, P2, P3, P4
3 resources: A(7 instance), B(2 instance), C(6 instance)
![[Pasted image 20250515163317.png|300]]
=> Request를 Need라 생각하고 풀면 된다.
• 시퀀스 <P0, P2, P3, P1, P4>는 모든 i에 대해 `Finish[i] = true`가 되는 결과를 낳습니다
• 만약 P2가 Request 0 0 1 을 추가로 요청하면?
	=> P1, P2, P3, P4가 관련된 교착 상태가 발생
## 교착 상태 복구
•	프로세스 종료
	•	모든 교착 상태의 프로세스를 종료
	•	교착 상태의 사이클이 제거될 때까지 하나씩 종료
		•	선택 기준은?
			•	프로세스의 우선순위
			•	얼마나 계산했는지와 완료까지 얼마나 남았는지
			•	프로세스가 사용한 자원
			•	완료에 필요한 자원
			•	종료해야 할 프로세스 수
			•	프로세스가 인터랙티브인지 배치인지
•	자원 선점
	•	고려해야 할 사항
		•	희생자 선택 – 비용 최소화
		•	롤백 – 안전 상태로 되돌아가 그 상태부터 재시작
		•	기아 – 동일한 프로세스가 항상 희생자가 되는 상황을 방지해야 하며, 롤백 횟수를 비용에 포함