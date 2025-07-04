## 기본 개념
Ordered Index(정렬된 인덱스)
B+ 트리 인덱스 파일
B-트리 인덱스 파일
## 동기
- 데이터베이스에서 인덱스란 무엇인가?
	- 간단히 말해, 원하는 레코드를 효율적으로 찾기 위한 잘 설계된 자료 구조입니다.
	- 왜 이렇게 다양한 인덱스 유형이 존재하는가?
		- 다양한 데이터 타입
		- 다양한 쿼리 유형
	- 매우 간략히 말하면, 데이터 엔지니어가 하는 일은 기본 데이터에 적절한 인덱스를 설계하고 사용하는 것입니다.
	- 그렇다면 왜 인덱스가 필요한가?
	- 다음과 같은 쿼리를 효율적으로 수행하기 위해서입니다:
```sql
select name, dept_name, salary  
from instructor  
where ID = 15151;
```
## 기본 개념
- 인덱싱 메커니즘은 원하는 데이터 접근을 빠르게 하기 위해 사용됩니다.
	- 예: 도서관의 저자 색인
- Search Key(검색 키): 파일 내 레코드를 찾기 위해 사용하는 속성(또는 속성 집합)
- 인덱스 파일은 다음 형식의 레코드(인덱스 항목)들로 구성됩니다:
	- search-key pointer
- 인덱스 파일은 일반적으로 원본 파일보다 훨씬 작습니다.
- 인덱스의 기본 유형 두 가지:
	1. Ordered indices (정렬 인덱스): 검색 키가 정렬된 순서로 저장됨
	2. Hash indices (해시 인덱스): 해시 함수를 이용해 키가 “버킷”에 균일하게 분산됨
## 인덱스 평가 지표
•	효율적으로 지원하는 접근 방식
	예: 특정 값을 가진 레코드, 또는 값 범위 내에 있는 레코드
•	접근 시간
•	삽입 시간
•	삭제 시간
•	공간 오버헤드

이론적으로 인덱스 성능을 분석하는 방법: 외부 메모리 모델 (강의노트 참고)
## Ordered Indices (정렬 인덱스)

*ordered index*는 검색 키 값을 기준으로 정렬된 순서로 index 항목을 저장합니다. 예: 도서관 저자 색인
•	Primary index (기본 인덱스): 파일 자체가 정렬된 순서대로 저장되어 있으며, 이 순서를 결정하는 검색 키를 가진 인덱스 (*clustering index*라고도 함)
	•	보통 기본 키가 검색 키지만, 반드시 그렇지는 않음
•	*Secondary index* (보조 인덱스): 파일의 순서와는 다른 순서로 검색 키를 정렬한 인덱스 (**non-clustering index**라고도 함)
•	*Index-sequential file*: ordered sequential file with a primary index.
예시: 	전화번호부: 이름순으로 정렬되어 있고, 이름 첫 글자별로 인덱스를 가진다면,
•	A~C → 첫 페이지
•	D~F → 중간 페이지
•	이런 식으로 인덱스가 있으면 특정 이름을 빠르게 찾아갈 수 있음
=> Index-sequential file이면, clustering index임

- In an *ordered index*, index entries are stored sorted on the search key value, e.g., author catalog in library.
- *Primary index*: in a sequentially ordered file, the index whose search key specifies the sequential order of the file. Also called *clustering index*
	- The search key of a primary index is usually but not necessarily the primary key.
- *Secondary index*: an index whose search key specifies an order different from the sequential order of the file. Also called *non-clustering index*.
- *Index-sequential file*: ordered sequential file with a *primary index*.
#### Q. Primary Index와 secondary Index는 항상 Dense Index인가?
Index-seqential file은 보통 sparse이고 dense여도 될 것은 같긴하다..
## Dense Index Files
![[Pasted image 20250513142647.png|300]]
•	Dense Index Files: 파일 내 모든 검색 키 값에 대해 index record가 존재
	•	예: instructor 테이블의 ID 속성에 대한 인덱스
Dense index— Index record appears for every search-key value in the file.
=> 보통 secondary index에서만 dense index를 사용

![[Pasted image 20250513142754.png|300]]
- dense index on dept_name with instructor file sorted on dept_name
- ***이것도 dense index임***!
## Sparse Index Files
• **Sparse Index**: 일부 검색 키 값에 대해서만 인덱스 레코드를 포함함
	– 검색 키를 기준으로 레코드가 순차적으로 정렬되어 있을 때 사용 가능
• 검색 키 값이 K인 레코드를 찾기 위해서는:
	– K보다 작거나 같은 검색 키 값 중 가장 큰 값을 가진 인덱스 레코드를 찾음
	– 해당 인덱스 레코드가 가리키는 레코드부터 파일을 순차적으로 검색함
![[Pasted image 20250513143209.png|300]]

• Dense Index와 비교할 때:
– 공간을 덜 차지하고, 삽입 및 삭제 시 유지 관리 비용이 더 적다.
– 일반적으로 레코드 검색 속도는 Dense Index보다 느리다.

• 좋은 절충안: 해당 블록에서 가장 작은 검색 키 값에 대응하는, 파일의 각 블록마다 하나의 인덱스 항목을 가지는 Sparse Index
=> 어차피 DB는 block단위로 data를 가져오기 때문
dense Index: N 개 항목, 블록당 B개 :O(N/B) blocks
sparse Index: N/B 개 항목, 블록당 B개: O(N/B<sup>2</sup>)
우리는 index도 block 단위로 가져올거임 index는 N/B개가 있고, N/B개를 블록단위로 가져오면 B개씩 가져올 수 있음 따라서 index를 블록 단위 접근하면 N/B<sup>2</sup>인 것
![[Pasted image 20250513143231.png|200]]
#### sparse index를 이용한 cost 계산 example
N<sub>data</sub> = 20,000,000 (20 M)
B<sub>data</sub> = 20
B<sub>index</sub> = 100 이라 가정 (Block size는 동일하지만 index는 key와 pointer만 들어가면 되므로)

N<sub>index</sub> = 1,000,000 (1M) 개의 block 존재 (data block마다 하나씩 있으므로)
index block = N<sub>index</sub>/B<sub>index</sub> = 10000
이분 탐색을 이용한 cost: O(log n) = 13.xxx = 14번 I/O
N이 data를 저장한 block의 개수일 때, 
N<sub>index</sub>/2<sup>i</sup> = B<sub>index</sub>
O(log<sub>2</sub>(N<sub>index</sub>/B<sub>index</sub>))

dense라면,O(log<sub>2</sub>(N<sub>data</sub>/B<sub>index</sub>))
#### 문제: Sparse Index를 사용한 탐색 비용 계산
한 데이터베이스에서 아래와 같은 정보가 주어졌다고 하자:
	•	전체 레코드 수: N<sub>data</sub> = 40,000,000 (40M)
	•	데이터 블록당 레코드 수: B<sub>data</sub> = 40
	•	Sparse Index는 각 데이터 블록의 첫 레코드만을 인덱싱
	•	인덱스 항목은 (key, pointer) 형식이고, 한 블록에 인덱스 항목 B<sub>index</sub> = 80개 저장 가능
	•	전체 인덱스 항목 수는 데이터 블록 수와 같음
	•	따라서 인덱스 블록 수 = N<sub>block</sub> / B<sub>index</sub>
	•	디스크 I/O는 모두 Block 단위로 계산

❓질문:
위 설정에서 Sparse Index를 이용하여 특정 레코드를 검색할 때,
필요한 최악의 경우 디스크 I/O 횟수는 총 몇 번인가?

✅ 풀이
1.	데이터 블록 수 (Nblock) 계산
	N_block = 40,000,000 / 40 = 1,000,000
2.	인덱스 항목 수 = 1,000,000 (각 데이터 블록당 1개)
	인덱스 블록 수: index_blocks = 1,000,000 / 80 = 12,500
3.	Sparse Index 이진 탐색 시 I/O 횟수
	인덱스 블록에서 이진 탐색할 경우, I/O는 블록 단위로 발생하므로: index_IO = log2(12,500) ≈ 13.6 → 14 I/Os
4.	찾은 인덱스 항목이 가리키는 데이터 블록 접근 1번: data_IO = 1
5.	총 디스크 I/O = index_IO + data_IO = 14 + 1 = 15
✅ 정답:
최악의 경우 디스크 I/O 횟수: 15번
## Multilevel Index
- 기본 인덱스가 메모리에 맞지 않으면 접근 비용이 증가
- 해결책:
	•	디스크상의 Primary index를 정렬된 파일로 보고, 그 위에 sparse index를 구축
	outer index – a sparse index of primary index
	inner index – the primary index file

- outer index도 메모리에 안 들어갈 경우, 더 상위 단계 인덱스를 생성 가능
- 삽입/삭제 시 모든 수준의 인덱스를 갱신해야 함

![[Pasted image 20250515140224.png|300]]
h = O(log<sub>B</sub>N) (B 꼭 써줘야 함 B가 상수가 아니므로)
N = B<sup>h</sup>
multilevel index의 문제: 인덱스에 레코드가 하나씩만 들어가게 되면 인덱스의 장점을 활용하지 못한다. (update하다보면 그럴 수도 있음)
## 인덱스 갱신: 삭제
![[Pasted image 20250520134904.png|300]]
•	삭제된 레코드가 해당 검색 키 값을 가진 유일한 레코드였다면, 인덱스에서도 해당 검색 키 삭제

Single-level index entry deletion
•	Dense Index: 파일 레코드 삭제와 유사
•	Sparse Index:
	• 인덱스에 검색 키에 대한 항목이 존재하면, 해당 항목은 파일에서 다음 검색 키 값(검색 키 순서)을 인덱스에 대신 넣어 삭제된다.
	• 만약 다음 검색 키 값이 이미 인덱스에 존재한다면, 항목은 교체되는 대신 삭제된다.
## 인덱스 갱신: 삽입
Single level index 삽입:
	삽입할 레코드에 나타나는 검색 키 값을 사용하여 탐색을 수행한다.
	Dense 인덱스 – 검색 키 값이 인덱스에 없다면 삽입한다.
	Sparse 인덱스 – 인덱스가 파일의 각 블록에 대한 항목만 저장하는 경우, 새로운 블록이 생성되지 않는 한 인덱스를 변경할 필요가 없다.
		• *새로운 블록이 생성되는 경우, 해당 블록에 나타나는 첫 번째 검색 키 값을 인덱스에 삽입한다.*
다단계 삽입 및 삭제: 알고리즘은 단일 수준 알고리즘의 단순한 확장이다.
## 보조 인덱스 (Secondary Indices)
자주, 어떤 필드(기본 인덱스의 검색 키가 아닌)의 값이 특정 조건을 만족하는 모든 레코드를 찾고 싶을 때가 있다.
예시 1: ID 기준으로 순차 저장된 instructor 릴레이션에서, 특정 학과에 속한 모든 교수를 찾고 싶은 경우
예시 2: 위와 동일하지만, 특정 급여를 받는 교수나 특정 범위의 급여를 받는 모든 교수를 찾고 싶은 경우

이런 경우, 각 검색 키 값마다 하나의 인덱스 레코드를 갖는 secondary index를 사용할 수 있다.
![[Pasted image 20250526145117.png|300]]
인덱스 레코드는 해당 검색 키 값을 가진 모든 실제 레코드에 대한 포인터들을 담고 있는 버킷(bucket)을 가리킨다.
•	보조 인덱스는 dense 인덱스여야 한다.
	sparse이면 1대1 대응이 되지 않기 때문
## Primary and Secondary Indices
•	인덱스는 레코드 검색 시 큰 이점을 제공
•	단점: 인덱스를 업데이트해야 하므로 DB 수정 시 오버헤드 발생
	•	파일이 수정될 때, 해당 파일의 모든 인덱스도 수정 필요
•	기본 인덱스를 이용한 순차 스캔은 효율적, 그러나 보조 인덱스를 이용한 순차 스캔은 비효율적
	•	각 레코드 접근 시 디스크에서 블록을 새로 불러올 수 있음
	•	디스크 접근 시간: 약 5~10밀리초, 메모리 접근 시간: 약 100나노초
보조 인덱스를 이용한 순차 스캔: O(N<sub>data</sub>/B<sub>data</sub>log<sub>B<sub>data</sub></sub>N<sub>data</sub>/B<sub>data</sub>)
## B+ 트리 인덱스 파일
B+ 트리는 *indexed-sequential files의 대안*입니다.
인덱스 순차 파일의 단점:
	•	파일이 커질수록 성능이 저하됩니다. 이는 많은 오버플로 블록이 생성되기 때문입니다.
	•	전체 파일을 주기적으로 재구성해야 합니다.
B+ 트리 인덱스 파일의 장점:
	•	삽입과 삭제가 발생하더라도 소규모의 국소적인 변경만으로 자동으로 스스로를 재구성합니다.
	•	성능을 유지하기 위해 전체 파일을 재구성할 필요가 없습니다.
B+ 트리의 (작은) 단점:
	•	삽입 및 삭제 시 추가적인 오버헤드와 공간 사용이 발생합니다.
그러나 B+ 트리의 장점은 단점을 상쇄하고도 남습니다.
	•	실제로 B+ 트리는 널리 사용됩니다.
#### example of B+ Tree
![[Pasted image 20250520140505.png|400]]
leaf node가 dense index인 예제
리프노드는 데이터를 가리키고 있음 
## 왜 B+ 트리인가?
모든 관계형 데이터베이스 시스템은 B+ 트리를 지원합니다.
지원 DBMS 예:
	•	IBM DB2, Informix, MS SQL, Oracle, Sybase, SQLite, MySQL, PostgreSQL, Tibero 등
	다른 인덱스 유형도 지원하지만, 가장 널리 사용되는 인덱스는 B+ 트리와 그 변형(B-트리, B*-트리 등) 입니다.
거의 모든 파일 시스템도 B+ 트리를 사용합니다.
	예: NTFS, EXT4, ReiserFS, NSS, XFS, JFS, ReFS, BFS 등
### DB에서 B+ Tree를 B Tree 대신 사용하는 이유
모든 레코드가 리프 노드에만 저장되어 리프 노드에 데이터가 정렬된 상태로 모여 있어 범위 검색이나 순차 접근에 최적화됨.
## B+ Tree 노드 개수
- root node: `[2, B]`
	- root가 leaf: 
		- ptrs ∈ `[1, B]`
- leaf node: (포인터가 귀찮게 생김)
	- search keys ∈ `[⌈(B–1)/2⌉, B-1]`
	- ptrs ∈ `[⌈(B–1)/2⌉ + 1, B]`
- non-leaf node: 
	- keys ∈ `[⌈B/2⌉-1, B-1]`
	- ptrs ∈ `[⌈B/2⌉, B]`
B: 하나의 노드에 들어가는 entry 개수
## B+ 트리의 정의
B+ 트리는 다음 조건을 만족하는 루트 트리입니다:
	•	*루트에서 리프까지의 모든 경로의 길이는 동일합니다.*
	•	루트나 리프가 아닌 각 노드는 최소 ⌈B/2⌉개에서 최대 n개의 자식 노드를 가집니다.
	•	리프 노드는 최소 ⌈(B–1)/2⌉개에서 최대 n–1개의 값을 가집니다.
특수한 경우:
	•	루트가 리프가 아닐 경우, 최소 2개의 자식을 가집니다.
	•	루트가 리프인 경우(트리에 다른 노드가 없을 때), 0개에서 B–1개 사이의 값을 가질 수 있습니다.
## B+ 트리 노드 구조
일반적인 노드:
	![[Pasted image 20250526160625.png|300]]
	•	Kᵢ는 검색 키 값입니다.
	•	Pᵢ는 자식 노드(비리프일 경우) 또는 레코드/레코드 버킷(리프일 경우)을 가리키는 포인터입니다.
•	노드 내 검색 키들은 정렬되어 있음: K₁ < K₂ < K₃ < ... < Kₙ₋₁
(초기에는 중복 키는 없다고 가정)
## 리프 노드의 특성
•	i = 1, 2, …, B–1에 대해, 포인터 Pᵢ는 검색 키 값 Kᵢ를 가진 파일 레코드를 가리킴
•	리프 노드 Lᵢ, Lⱼ에서 i < j이면, Lᵢ의 키 값들은 Lⱼ의 키 값들보다 작거나 같음
•	*P<sub>B</sub>은 다음 리프 노드를 가리킴 (검색 키 순서로)*
![[Pasted image 20250526160654.png|300]]
## Non-Leaf 노드의 특성
•	비리프 노드는 리프 노드에 대한 다단계 sparse index를 형성함
	•	포인터가 m개인 비리프 노드에서:
		•	P₁이 가리키는 서브트리의 모든 키는 K₁보다 작음
		•	2 ≤ i ≤ B–1인 경우, Pᵢ가 가리키는 서브트리의 키는 Kᵢ₋₁ 이상이고 Kᵢ 미만
		•	P<sub>B</sub>이 가리키는 서브트리의 키는 K<sub>B-1</sub> 이상
## B+ 트리 예시
![[Pasted image 20250526160757.png|400]]
	•	instructor 파일에 대한 B+ 트리 예 (B = 6)
•	리프 노드는 3~5개의 값을 가짐 (⌈(B–1)/2⌉ = 3, B–1 = 5)
•	루트를 제외한 비리프 노드는 3~6개의 자식 (⌈B/2⌉ = 3, B = 6)
•	루트는 최소 2개의 자식을 가져야 함
## B+ 트리에 대한 관찰
•	노드 간 연결은 포인터로 이루어지므로, 논리적으로 가까운 블록이 물리적으로 가까울 필요는 없음
•	비리프 노드는 리프에 대한 계층적 sparse index를 형성함
•	B+ 트리는 상대적으로 적은 수의 레벨을 가짐:
	•	루트 바로 아래 수준: 최소 2 × ⌈B/2⌉ 개의 값
	•	그다음 수준: 최소 2 × ⌈B/2⌉ × ⌈B/2⌉
	•	…
•	N개의 검색 키가 있을 때, 트리의 높이는 최대 ⌈log<sub>⌈B/2⌉</sub>(N)⌉ 그냥 O(log<sub>B<sub>index</sub></sub>N)이라 생각하자
→ 따라서 검색은 매우 효율적입니다.
→ 삽입 및 삭제도 로그 시간 내에 처리 가능
## B+ 트리에서의 쿼리 처리
검색 키 값 V를 가진 레코드를 찾는다.
1.	C = root
2.	C가 리프 노드가 아닌 동안
		① V ≤ K<sub>i</sub>를 만족하는 가장 작은 i를 찾는다.
		② 그런 i가 없다면(자기가 제일 크다면), C에서 마지막으로 null이 아닌 포인터를 C로 설정한다.
		③ 그렇지 않다면 V = K<sub>i</sub>이면 C = P<sub>i+1</sub>로 설정하고, 그 외에는 C = P<sub>i</sub>로 설정한다
3.	K<sub>i</sub> = V를 만족하는 가장 작은 i를 찾는다.
4.	그런 값 i가 있다면, 포인터 Pi를 따라가 원하는 레코드를 찾는다.
5.	그렇지 않으면, 검색 키 값 V를 가진 레코드는 존재하지 않는다.
## 효율성 비교
•	K개의 검색 키가 있을 때, 트리 높이는 최대 ⌈log<sub>⌈B/2⌉</sub>(K)⌉
•	일반적으로 노드 하나는 디스크 블록 크기와 같고, 보통 4KB
	•	인덱스 항목 하나당 약 40바이트라고 하면, B ≈ 100
#### example
•	검색 키가 100만 개이고 B = 100이면,
•	최대 log₅₀(1,000,000) ≈ 4개의 노드만 접근 필요
비교:
	•	균형 이진 트리로는 약 20개의 노드를 접근해야 함
→ 디스크 I/O가 필요한 노드 접근이 적기 때문에 큰 성능 차이 발생
→ 디스크 접근은 약 20ms, 메모리 접근은 약 100ns로 차이가 큼
![[Pasted image 20250526160920.png|500]]
=> O(log<sub>B</sub>N) I/O's
### Range Search
range_search: O(log<sub>B</sub>N + T/B)
여기서 T는 range의 길이를 뜻함
![[Pasted image 20250527133631.png|500]]
만약, secondary index라면, O(log<sub>B</sub>N + T) I/O's
T scan시에 모두 다른 block에 존재할 수 있기 때문
## B+ 트리의 갱신: 삽입
1.	검색 키 값이 들어갈 리프 노드를 찾습니다.
2.	해당 키 값이 이미 리프 노드에 있다면:
	•	파일에 레코드를 추가합니다.
	•	필요하다면 버킷에 포인터를 추가합니다.
3.	검색 키 값이 리프 노드에 없다면:
	•	메인 파일에 레코드를 추가하고, 필요 시 버킷을 생성합니다.
	•	리프 노드에 공간이 있으면 (키 값, 포인터) 쌍을 삽입합니다.
	•	공간이 없다면, 노드를 분할합니다
### 리프 노드 분할
=> leaf와 non-leaf가 차이가 있다.
- 삽입하려는 항목을 포함하여 n개의 (검색 키 값, 포인터) 쌍을 정렬된 순서로 정리한다. 
- **처음 ⌈n/2⌉개의 쌍은 원래 노드에 저장하고, 나머지는 새 노드에 저장**한다. 
- **새 노드에 있는 가장 작은 키 값을 분할된 노드의 부모 노드에 삽입**한다.
- **부모 노드가 가득 차 있으면, 부모 노드도 분할하고 분할을 위쪽으로 전파**한다. 

- 이 작업은 가득 차지 않은 노드를 만날 때까지 위로 진행된다. 
	- 최악의 경우 루트 노드까지 분할되어 트리의 높이가 1 증가할 수 있다.
![[Pasted image 20250526162615.png|200]]
Brandt, Califieri, Crick를 포함한 노드에 Adams를 삽입하여 분할한 결과:
다음 단계는 (Califieri, 새 노드에 대한 포인터)를 부모 노드에 삽입하는 것이다.
#### Example of B+ Tree Insertion
![[Pasted image 20250526162644.png|500]]
Adams 삽입 전후 B+ Tree 

![[Pasted image 20250526162729.png|500]]
Lamport 삽입 전후 B+ Tree
leaf node는 value를 기준으로 나누고, non-leaf node는 pointer를 기준으로 나눈다.
Gold가 root까지 올라가는 과정을 잘 보자
### non-leaf node 분할
• 이미 가득 찬 내부 노드 N에 (k, p)를 삽입할 때
	• N을 n+1개의 포인터와 n개의 키를 저장할 수 있는 메모리 영역 M으로 복사한다
	• (k, p)를 M에 삽입한다
	• M에서 P₁, K₁, …, K<sub>⌈n/2⌉-1</sub>, P<sub>⌈n/2⌉</sub>를 다시 노드 N에 복사한다
	• M에서 P<sub>⌈n/2⌉+1</sub>, K<sub>⌈n/2⌉+1</sub>, …, Kₙ, Pₙ₊₁를 새로 할당한 노드 N′에 복사한다
	• (K<sub>⌈n/2⌉</sub>, N′)을 부모 노드 N에 삽입한다 (leaf에서는 유지하고 올라가던 애가 non-leaf에서는 없어지고 올라감)
![[Pasted image 20250603184400.png]]
###### Lecture Board Example
![[Pasted image 20250526192116.png|300]]
이 상황에서 11을 넣는다면?

leaf node와 non-leaf node의 범위는?
- leaf node : 
	- vals ∈ \[2, 3]
	- ptrs  ∈ \[3, 4]
- non-leaf node
	- ptrs  ∈ \[2, 4]

![[Pasted image 20250526192326.png|300]]
1. 일단 삽입하려는 곳이 full이므로 분할함
![[Pasted image 20250526192409.png|300]]
2. 분할되었기 때문에 삽입이 parent node로 전파됨
#### pseudo code
![[Pasted image 20250526192511.png|400]]
find_leaf 시간복잡도: O(log<sub>B</sub>N)
split 시간 복잡도: O(1) => 해봤자 노드 하나를 가져와서 나누는 것 뿐이므로 
splits => h번 = O(log<sub>B</sub>N)
따라서 삽입 시간 복잡도: O(log<sub>B</sub>N)
#### Example 2
leaf node에서는 value를 기준으로 나누었지만, non-leaf node에서는 ptr을 기준으로 나눈다.
![[Pasted image 20250526193100.png|400]]

![[Pasted image 20250526195040.png|400]]
왼쪽에 ptr 3개, 오른쪽에 2개를 놓고 붕 뜬 value인 100은 위로 올린다.

#### Example of B+ Tree Deletion
![[Pasted image 20250526163138.png|500]]
•	“Srinivasan” 삭제 전후: 

![[Pasted image 20250526163155.png|500]]
삭제로 인해 리프 노드가 부족해 병합 발생 (non-leaf에서 Srinivasan->Mozart로 변경되는 과정 확인)
1. leaf node에서 Srinivasan 삭제
2. Wu만 남아서 최소 개수 충족하지 못함 => 재분배 또는 병합 => 옆에 자리가 있으니 병합
3. non-leaf node에서도 Srinivasan이 삭제되어 최소 개수 충족하지 못함 => 넘어감...
###### other example
![[Pasted image 20250526163245.png|500]]
•	“Singh”와 “Wu” 삭제: 

리프 노드가 부족해 왼쪽 형제 노드로부터 “Kim”을 빌려옴
→ 부모 노드의 키 값 변경

![[Pasted image 20250526163310.png|500]]
•	“Gold” 삭제 전후:
•	“Gold”와 “Katz”가 있는 노드가 부족해 형제와 병합됨
•	부모 노드도 부족해 형제와 병합됨
•	부모 노드의 키가 아래로 내려옴
•	루트 노드에 자식이 하나만 남으면, 루트는 삭제되고 해당 자식이 새 루트가 됨
## B+ 트리의 갱신: 삭제
Otherwise, if the node has too few entries due to the removal, but the entries in the node and a sibling do not fit into a single node, then **redistribute pointers**
- 삭제할 레코드를 찾아 메인 파일과 버킷(존재할 경우)에서 제거한다.
- 버킷이 없거나 버킷이 비게 된 경우, 리프 노드에서 (검색 키 값, 포인터)를 제거한다.
- 삭제로 인해 노드에 항목이 너무 적어졌고, 해당 노드와 형제 노드의 항목을 하나의 노드에 넣을 수 있다면, *형제 노드를 병합한다*:
	- 두 노드의 모든 검색 키 값을 하나의 노드(왼쪽 노드)에 삽입하고, 다른 노드를 삭제한다.
	- 삭제된 노드를 가리키는 포인터 Pi에 대한 (Ki–1, Pi) 쌍을 부모 노드에서 제거하고, 위 절차를 재귀적으로 적용한다.
- 그렇지 않고, 삭제로 인해 노드에 항목이 너무 적어졌지만 형제 노드와 병합할 수 없는 경우에는 *포인터를 재분배*한다:
	- 노드와 형제 노드 간에 포인터를 재분배하여 두 노드 모두 최소 항목 수 이상을 유지하도록 한다.
	- 노드의 부모 노드에 있는 해당 검색 키 값을 갱신한다.
- 노드 삭제는 상위 노드로 연쇄되어 진행될 수 있으며, 최소 ⌈n/2⌉ 개 이상의 포인터를 가진 노드를 만날 때까지 계속된다.
- 루트 노드가 삭제 후 포인터 하나만 남는다면, 해당 루트 노드는 삭제되고 그 자식 노드가 새로운 루트가 된다.
## B+ 트리 파일 구성
- 인덱스 파일 성능 저하 문제는 B+ 트리 인덱스를 사용하여 해결할 수 있다.
- 데이터 파일 성능 저하 문제는 B+ 트리 파일 구성 방식을 통해 해결된다.
- B+ 트리 파일 구성에서는 리프 노드에 포인터 대신 실제 레코드를 저장한다.
- 리프 노드도 여전히 절반 이상 차 있어야 한다.
	- 레코드는 포인터보다 크기 때문에, 리프 노드에 저장할 수 있는 최대 레코드 수는 비리프 노드에서 포인터를 저장할 수 있는 수보다 적다.
- 삽입과 삭제는 B+ 트리 인덱스에서의 삽입 및 삭제 방식과 동일하게 처리된다.

![[Pasted image 20250526163813.png|400]]
•	포인터보다 레코드가 더 많은 공간을 차지하므로 공간 활용이 매우 중요하다.
•	공간 활용을 개선하기 위해 분할 및 병합 시 더 많은 형제 노드를 재분배에 참여시킨다.
•	가능한 한 분할/병합을 피하기 위해 2개의 형제 노드를 재분배에 포함시키는 방식은, 각 노드가 최소 ⌊2B/3⌋개의 항목을 갖도록 한다. (원래 2B가 꽉차있는 것을 3개로 나누었으므로)
## 인덱싱의 기타 이슈들
- 레코드 재배치와 보조 인덱스
	- 레코드가 이동하면, 해당 레코드를 가리키는 모든 보조 인덱스를 업데이트해야 한다.
	- B+ 트리 파일 구성에서의 노드 분할은 매우 비용이 크다.
	- 해결책: 보조 인덱스에서 레코드 포인터 대신 기본 인덱스의 검색 키를 사용한다.
		•	레코드를 찾기 위해 기본 인덱스를 한 번 더 탐색해야 한다.
			•	쿼리 비용은 증가하지만, 노드 분할 비용은 낮다.
		•	기본 인덱스의 검색 키가 유일하지 않다면 레코드 ID를 추가한다.
## B-트리 인덱스 파일
![[Pasted image 20250526163849.png|300]]
B+ 트리와 유사하지만, B-트리는 검색 키 값이 한 번만 나타나므로 중복된 키 저장을 피할 수 있다.
비리프 노드의 검색 키 값은 트리의 다른 곳에 나타나지 않는다.
비리프 노드의 각 검색 키에 대해 추가적인 포인터 필드가 포함되어야 한다.
	•	일반화된 B-트리의 leaf node (a)
	![[Pasted image 20250526163903.png|300]]
	•	nonleaf node(b): Bi 포인터는 버킷 또는 파일 레코드 포인터를 가리킨다.
#### Example
![[Pasted image 20250526163936.png|500]]
포인터를 두 개씩 가지게 된다.
### B-트리 인덱스의 장점:
•	동일한 B+-트리보다 적은 수의 트리 노드를 사용할 수 있다.
•	리프 노드에 도달하기 전에 검색 키 값을 찾는 것이 가능할 수 있다.
### B-트리 인덱스의 단점:
•	전체 검색 키 값 중 일부만 조기 탐색 가능하다.
•	비리프 노드가 더 커서 fan-out(가지 수)이 줄어든다.

→ 따라서 B-트리는 일반적으로 B+-트리보다 깊이가 더 크다.
	•	삽입과 삭제가 B+-트리보다 더 복잡하다.
	•	구현이 B+-트리보다 더 어렵다.
일반적으로 B-트리의 장점은 단점을 상쇄하지 못한다.
## 다중 키 접근 (Multiple-Key Access)
특정 유형의 질의(query)에 대해 여러 개의 인덱스를 사용할 수 있다.
예시:
```sql
select ID  
from instructor  
where dept_name = "Finance" and salary = 80000
```
단일 속성에 대한 인덱스를 사용하여 질의를 처리할 수 있는 가능한 전략들:
1.	dept_name에 대한 인덱스를 사용하여 부서 이름이 “Finance”인 강사를 찾고, 이후 salary = 80000인지 검사한다. (Fincance인 강사가 적을 때 유리)
2.	salary에 대한 인덱스를 사용하여 급여가 80000인 강사를 찾고, 이후 dept_name = “Finance”인지 검사한다. (급여가 80000인 강사가 적을 때 유리)
3.	dept_name 인덱스를 사용하여 “Finance” 부서에 해당하는 모든 레코드 포인터를 찾고, salary 인덱스를 사용하여 급여가 80000인 레코드 포인터를 찾은 후, 두 포인터 집합의 교집합을 구한다.
## 다중 키에 대한 인덱스 (Indices on Multiple Keys)
*composite search key*는 둘 이상의 속성을 포함하는 검색 키이다.
- 예: (dept_name, salary)
사전식(lexicographic) 정렬:
- (a1, a2) < (b1, b2) 이려면 다음 중 하나를 만족해야 한다:
	•	a1 < b1
	•	또는 a1 = b1 이고 a2 < b2
## Indices on Multiple Attributes
- 복합 검색 키 (dept_name, salary)에 대한 인덱스가 있다고 가정하자.
- 조건절이 다음과 같을 경우:  `where dept_name = "Finance" and salary = 80000`
- (dept_name, salary)에 대한 인덱스를 사용하면 두 조건을 모두 만족하는 레코드만 가져올 수 있다.
	- 별도의 인덱스를 사용할 경우 비효율적일 수 있는데, 한 조건만 만족하는 많은 레코드(또는 포인터)를 가져오게 될 수도 있기 때문이다.
- 다음 조건도 효율적으로 처리할 수 있다:
	- `where dept_name = "Finance" and salary < 80000`
- 하지만 다음과 같은 조건은 효율적으로 처리할 수 없다:
	- `where dept_name < "Finance" and balance = 80000`
	- 이 경우 첫 번째 조건을 만족하지만 두 번째 조건을 만족하지 않는 많은 레코드를 가져오게 될 수 있다.