## 가상 메모리의 동기
•	코드와 데이터를 실행하려면 메모리에 있어야 하지만, 전체 프로그램이 항상 사용되는 것은 아니다
	•	오류 처리 코드, 특이한 루틴, 큰 데이터 구조 등은 드물게 사용됨
•	partially-loaded program을 실행할 수 있다면 좋음
	•	프로그램이 물리적 메모리 용량의 제약을 받지 않게 됨
	•	실행 중인 각 프로그램이 적은 메모리를 차지함 → 동시에 더 많은 프로그램 실행 가능
		•	응답 시간이나 반환 시간의 증가 없이 CPU 활용률과 처리량이 향상됨
	•	프로그램을 메모리에 적재하거나 교체(swap)하는 데 필요한 I/O가 줄어듦(필요한 것만 가져다 두기 때문) → 사용자 프로그램이 더 빠르게 실행됨
## 가상 메모리
•	사용자 논리 메모리와 물리 메모리의 분리
	•	실행을 위해 프로그램의 일부만 메모리에 존재하면 됨
	•	논리 주소 공간이 물리 주소 공간보다 훨씬 클 수 있음
	•	여러 프로세스가 주소 공간을 공유 가능 (프로그램마다 주소에 똑같은 규칙을 적용해도 되기 때문)
		•	프로세스 생성이 더 효율적임
		•	더 많은 프로그램을 동시에 실행 가능
		•	프로세스를 적재하거나 교체하는 데 필요한 I/O 감소
•	가상 주소 공간: 프로세스가 메모리에 저장된 방식에 대한 논리적 관점
	•	일반적으로 주소 0부터 시작해서 연속된 주소를 가짐
	•	물리 메모리는 페이지 프레임 단위로 구성됨
	•	MMU가 논리 주소를 물리 주소로 매핑해야 함
•	가상 메모리는 다음 방식으로 구현 가능
	•	Demand Paging
	•	Demand Segmentation
•	가상 메모리는 물리 메모리보다 크다
![[Pasted image 20250529165423.png|200]]

•	가상 주소 공간
	•	코드에서 스택까지 0에서 최대까지의 연속된 메모리 공간
	•	스택과 힙 사이에 사용되지 않는 주소 공간 존재
	•	시스템 라이브러리는 가상 주소 공간에 매핑하여 공유됨
	•	페이지를 읽기-쓰기 형태로 가상 주소 공간에 매핑하여 공유 메모리 구현 가능
	•	fork() 시 페이지 공유를 통해 프로세스 생성 속도 향상
•	가상 메모리를 이용한 공유 라이브러리
![[Pasted image 20250529165445.png|200]]
## Demand Paging
paging과 virtual memory가 다른 점인, 일부 데이터만 올리기 위한, 기본적인 방법
## Demand Paging의 기본 개념
![[Pasted image 20250529165803.png|200]]
•	필요한 페이지만 메모리에 불러옴
	•	불필요한 I/O 없음 → 더 적은 I/O
	•	더 적은 메모리 사용
	•	응답 시간 단축 (거대한 데이터를 가져오지 않아도 되니까)
	•	더 많은 사용자 수용 (메모리 공간이 줄어드니까)
•	스와핑을 사용하는 페이징 시스템과 유사함
	•	스와핑은 전부 빼고 전부 가져왔는데, 이건 페이지 단위로 빼고 가져온다.
•	페이지가 필요함 → 참조 발생 (해당 page에 접근할 때, 가져오면 된다.)
	•	잘못된 참조 → 중단 (얘가 사용하는 메모리 공간인지 체크)
	•	메모리에 없으면 → 메모리로 불러오기 (유효한 메모리라면, 그 페이지만 메모리로 가져온다)
•	Lazy Swapper: 실제로 필요한 페이지만 메모리에 적재
	•	페이지를 다루는 swapper는 pager라고 함
•	Demand Paging을 구현하려면 MMU에 새로운 기능이 필요
•	필요한 페이지가 이미 메모리에 있다면
	•	일반 페이징과 차이 없음
•	필요한 페이지가 메모리에 없다면
	•	저장 장치에서 해당 페이지를 감지하고 불러와야 함
		•	프로그램 동작을 바꾸지 않아야 함
		•	프로그래머가 코드를 변경할 필요 없음
## Valid-Invalid Bit
•	각 페이지 테이블 항목마다 유효-무효 비트를 둔다
	•	v → 메모리에 존재 (유효)
	•	i → 메모리에 없음 (무효)
•	처음에는 모든 항목의 비트를 i로 설정
•	예시: 페이지 테이블 스냅샷
![[Pasted image 20250529170510.png|150]]
•	MMU가 주소 변환 시 해당 비트가 i이면 → 페이지 폴트 발생
## Page Table Example
![[Pasted image 20250529170535.png|300]]
## Page Fault
•	페이지 참조가 발생하면, 해당 페이지에 대한 첫 참조 시 운영체제로 trap 발생 (page fault)
1.	운영체제가 다른 테이블을 확인함
	•	잘못된 참조 → 중단
	•	단지 메모리에 없을 경우 다음 step
2.	빈 프레임 찾기
3.	disk operation을 통해 해당 페이지를 프레임에 스와핑 (swap in)
4.	페이지가 메모리에 있다고 테이블 갱신, valid-invalid bit를 v로 세팅
5.	페이지 폴트를 유발한 명령어를 재시작
큰 흐름은 이렇게 됨

![[Pasted image 20250529171418.png|400]]
## Demand Paging의 특징
•	*pure demand paging*: 처음에는 아무 페이지도 메모리에 없이 프로세스 시작
	•	OS는 프로세스의 첫 명령어 위치로  instruction pointer를 설정 → 메모리에 없음 → 페이지 폴트
	•	다른 모든 페이지들도 처음 접근 시 페이지 폴트
•	실제로는 하나의 명령어가 여러 페이지를 접근할 수도 있음 → 다중 페이지 폴트
	•	예시: 메모리에서 두 수를 더하고 결과를 저장하는 명령어의 fetch/decode 과정
	•	지역성(locality of reference) 때문에 이러한 비용이 완화됨
•	요청 페이징을 위한 하드웨어 지원 필요
	•	유효/무효 비트를 갖는 페이지 테이블 (MMU)
	•	secondary memory (swap device) - backing store
	•	Instruction restart
## 명령어 재시작
•	page fault가 명령어 중간에서 발생할 수 있기 때문에 프로세스를 계속 실행시키는 것은 매우 까다롭다
	•	사용자 프로세스는 페이지 폴트가 발생한 사실조차 몰라야 함
	•	해당 명령어를 건너뛰는 것은 불가능
		•	프로세스에 투명하지 않음
	•	명령어는 처음부터 다시 실행되어야 함
		•	자동 증가/감소 같은 명령어는 어떻게 처리할까?
	•	명령어 재시작을 위한 하드웨어 지원이 필요함
## TLB 오류 vs. 페이지 폴트
•	메모리 관련 오류는 두 가지가 있다:
	•	TLB 오류 (Miss) (메모리에 있는지 없는지는 모르겠고, 최근 힌트를 적어놓은 TLB에 번역결과가 없다는 뜻)
		•	가상 주소를 물리 주소로 변환하는 정보가 TLB에 없음
	•	페이지 폴트
		•	해당 가상 페이지의 내용이 초기화되지 않았거나 메모리에 없음
•	중요한 사실
	•	모든 페이지 폴트는 TLB 오류가 선행된다
		•	가상 페이지의 내용이 메모리에 없다면, 해당 주소에 대한 변환 정보가 존재할 수 없음
	•	모든 TLB 오류가 페이지 폴트를 유발하지는 않는다
		•	페이지가 이미 메모리에 있고, 변환 정보가 페이지 테이블에 존재한다면, 페이지 폴트 없이 TLB 오류만 처리 가능
## Demand paging의 단계
page fault 직후
1.	운영체제로 trap 발생 -> page fault handler가 돈다
2.	사용자 레지스터와 프로세스 상태 저장
3.	인터럽트가 페이지 폴트인지 확인
4.	참조된 페이지가 합법적인지 검사하고, 디스크 상의 위치를 결정
5.	디스크에서 빈 프레임으로 페이지를 읽기 위한 요청 실행
6.	대기하는 동안 CPU를 다른 사용자(프로세스)에게 할당
7.	디스크 I/O 하위 시스템에서 인터럽트 수신 (I/O 완료)
8.	다른 사용자(프로세스)의 레지스터와 상태 저장
9.	인터럽트가 디스크에서 발생했음을 확인
10. 페이지가 이제 메모리에 있음을 나타내도록 페이지 테이블 및 기타 테이블 갱신
	1. ready queue에 page fault를 야기한 process를 넣어준다
11. 이 프로세스에 다시 CPU가 할당될 때까지 대기
12. 사용자 레지스터, 프로세스 상태, 갱신된 페이지 테이블 복원 후 중단된 명령어 재시작
## 요청 페이징 성능
•	세 가지 주요 작업
	•	인터럽트 처리: 신중한 코딩 시 수백 개의 명령어로 처리 가능
	•	페이지 읽기 및 희생 페이지 쓰기 (스왑): 많은 시간 소요
	•	프로세스 재시작: 다시 소량의 시간 소요
•	페이지 폴트율 p: 0 ≤ p ≤ 1 (메모리 접근당 page fault 횟수)
	•	p = 0: 페이지 폴트 없음
	•	p = 1: 모든 접근이 페이지 폴트 발생
•	유효 접근 시간 (Effective Access Time, EAT)
	•	EAT = (1 - p) * memory access time + p * (page fault overhead + swap page out + swap page in)
수식을 막 외울 필요는 X
#### 예시
•	메모리 접근 시간 = 100ns
•	평균 페이지 폴트 처리 시간 = 8ms (페이지 폴트 오버헤드 + 스왑 시간)
•	EAT = (1 - p) * 100 + p * 8,000,000 = 100 + p * 7,999,900
•	만약 1,000번 중 1번이 페이지 폴트라면 (p = 0.001)
	•	EAT = 8.1μs (평균 80배가 커져버림)
•	성능 저하를 10% 미만으로 제한하고 싶다면 (EAT < 110ns)
	•	110 > 100 + p * 7,999,900
	•	10 > p * 7,999,900
	•	p < 0.00000125
	→ 약 80만 번의 메모리 접근당 1번만 페이지 폴트가 발생해야 함
그냥 page fault는 이정도로 영향이 크다 정도로 생각하면 된다.
# Page selection
수요 페이징의 핵심 이슈
• 페이지 선택
• 언제 어떤 페이지를 메모리에 가져올 것인가?
• 페이지 교체
• 어떤 페이지를 언제 버릴 것인가?
• 페이지 프레임 할당
• 각 프로세스에 몇 개의 프레임을 할당할 것인가?

페이지 선택 (1)
• 페이지 선택 정책
• 수요 페이징(Demand paging)
• 프로세스를 시작할 때 아무 페이지도 로드하지 않음
• 해당 페이지에 대한 페이지 폴트가 발생하면 그때 로드
• 반드시 메모리에 있어야 할 때까지 기다림
• 거의 모든 페이징 시스템이 이 방식 사용
• 사전 페이징(Prepaging)
• 참조되기 전에 페이지를 미리 메모리에 불러옴
• 한 페이지가 참조되면, 혹시 몰라 다음 페이지도 불러옴
• 예측 능력자 없이 효과적으로 하기 어려움
• 가끔 효과적임: 순차적 읽기 예측(read-ahead)
• 요청 페이징(Request paging)
• 사용자가 필요한 페이지를 직접 지정
• 이 방식의 문제점은?
• 사용자가 항상 최선의 선택을 하는 것은 아님
• 사용자가 객관적이지 않음 (필요 이상으로 과대 평가)

• Copy-on-Write
• Copy-on-Write(COW)는 부모와 자식 프로세스가 처음에는 동일한 페이지를 메모리에서 공유하도록 허용함
• 둘 중 어느 쪽이 공유된 페이지를 수정하면, 그때 비로소 해당 페이지를 복사함
• COW는 수정된 페이지만 복사하므로 프로세스를 보다 효율적으로 생성할 수 있음
• 일반적으로, 빈 페이지는 ‘요청 시 0으로 채워지는(zero-fill-on-demand)’ 페이지 풀에서 할당됨
• 빠른 수요 페이징 처리를 위해 이 풀에는 항상 여유 프레임이 있어야 함
• 페이지 폴트 발생 시 프레임을 해제하고 다른 처리를 동시에 하지 않도록 하기 위함
• 페이지를 할당하기 전에 왜 0으로 초기화하는가?
• vfork()는 fork() 시스템 호출의 변형으로, 부모는 대기하고 자식은 부모의 copy-on-write 주소 공간을 사용
• 자식이 exec()를 호출하도록 설계됨
• 매우 효율적임

• Copy-on-Write 예시: 프로세스 1이 페이지 C를 수정하기 전
• Copy-on-Write 예시: 프로세스 1이 페이지 C를 수정한 후
