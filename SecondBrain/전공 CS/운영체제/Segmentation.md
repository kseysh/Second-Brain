## Problem of Contiguous Allocation
• 프로세스당 하나의 세그먼트(파티션)만 허용됨
	• 개인 데이터 영역은 유지하면서 두 프로세스가 코드를 어떻게 공유할 수 있을까?
• 프로그램은 여러 세그먼트로 구성된 집합임
	• 세그먼트는 다음과 같은 논리적 단위임:
		• 메인 프로그램
		• 함수
		• 객체
		• 지역 변수, 전역 변수
		• 스택
		• 심볼 테이블
		• 배열
## Segmentation
메모리의 유저 뷰를 지원하는 메모리 관리 스키마
![[Pasted image 20250522164000.png|150]]
• 프로세스가 여러 메모리 영역에 나뉘어 존재하는 것을 허용
• 각 세그먼트에 대해 개별적인 base, limit register 쌍을 사용하며, two protection bits(읽기 및 쓰기)도 추가
• 각 메모리 참조는 세 가지 방식 중 하나 이상으로 세그먼트와 오프셋을 지정
	• 주소의 상위 비트는 세그먼트를 선택하고, 하위 비트는 오프셋을 나타냄 (4가지로 나누려면 상위 두 비트만 8가지로 나누려면 상위 세 비트 이런 식으로 1/n하는 것)
	• 명령어에 의해 암시적으로 세그먼트가 선택됨
		• 예시: 코드 vs. 데이터, 스택 vs. 데이터
	• 세그먼트 테이블은 프로세스의 모든 세그먼트에 대한 base와 limit를 저장
contiguous: process마다 base와 limit
segmentation: segment마다 base와 limit
## Logical View of Segmentation
![[Pasted image 20250522164213.png|300]]
이 방식은 데이터가 꼭 연속적으로 저장되지 않아도 된다.

## Segmentation Architecture
• 논리 주소는 두 개의 튜플로 구성됨
	• <segment-number, offset>
	• segment-number: 몇 번째 segment인지 식별
	• offset: 해당 세그먼트 내의 상대적인 위치
	![[Pasted image 20250522165318.png|200]]
• 세그먼트 테이블은 이차원 물리 주소를 매핑함
	• base - 해당 세그먼트가 메모리 내에 위치하는 시작 물리 주소
	• limit - 해당 세그먼트의 길이(허용되는 offset의 최대 값)
• 세그먼트 테이블 베이스 레지스터(STBR): 세그먼트 테이블의 물리 메모리 내 위치(시작 주소)를 가리킴
• 세그먼트 테이블 길이 레지스터(STLR): 세그먼트 테이블에 포함된 세그먼트 개수 (즉, segment number의 유효 범위)
	• segment number는 STLR보다 작을 때 유효함
테이블도 메모리 내에 있어야 한다.
contiguous: base, limit register 관리
segmentation STBR, STLR register 관리

![[Pasted image 20250522165648.png|400]]
1. STBR에서 segment table의 physical memory 내 위치를 찾는다
2. segment number (s)가 STLR보다 작은지 검사
	- 그렇지 않으면 trap 발생
3. segment number를 이용해 segment table에서 base와 limit 값을 가져옴
4. offset (d)이 limit보다 작은지 검사
	- 그렇지 않으면 trap 발생
5. base + offset을 계산해 physical address 구함

![[Pasted image 20250522170139.png|400]]