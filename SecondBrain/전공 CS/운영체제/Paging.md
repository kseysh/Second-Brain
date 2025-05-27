## 페이징의 동기
• 세그멘테이션의 문제점
	• 외부 단편화
• 페이징의 목적
	• 고정 크기 메모리 블록을 사용해 할당 및 스와핑을 더 쉽게 수행
	• 메모리 단편화를 줄이기 위함
## 페이징(Paging)
• 프로세스의 물리 주소 공간은 연속적이지 않아도 됨
• 물리 메모리를 고정 크기의 블록으로 나누며 이를 프레임(*frame*)이라 함 (physical page)
	• 크기는 2의 거듭제곱, 512바이트 ~ 16MB 범위 (4KB)
• 논리 메모리도 동일 크기의 블록으로 나누며 이를 페이지(*pages*)라 함 (logical page)
• OS가 항상 사용 가능한 모든 frame을 추적
• N개의 *페이지*로 구성된 프로그램을 실행하기 위해 *N개의 빈 프레임*을 찾아 프로그램을 로드
• 논리 주소를 물리 주소로 변환하는 *페이지 테이블* 설정 (page마다 어떤 frame에 mapping되었는지)
• 여전히 내부 단편화 발생 가능
## 주소 변환 방식 (MMU)
• CPU가 생성한 주소는 다음과 같이 나뉨:
	• 페이지 번호(p): 해당 논리 페이지가 물리 메모리의 어느 프레임에 있는지 찾는 데 사용됨 (page table의 인덱스라고 생각)
	• 페이지 오프셋(d): 기본 주소와 결합되어 실제 물리 주소를 결정

• 논리 주소 공간이 2<sup>m</sup>이고 페이지 크기가 2<sup>n</sup>일 때 변환 수행
![[Pasted image 20250522171227.png|200]]
m이 32, n이 12인 것을 가정

전체 메모리 4GB는 32bit 주소 표현
page size 4KB (=frame)
frame 갯수 = 4GB/4KB = 2<sup>20</sup>개
p = 20 bit (2<sup>m</sup>/2<sup>n</sup>개여야 하므로)
d = 12 bit
**Q. page 수와 frame 수는 같은가?**
위 예시에서 page number는 2<sup>20</sup>까지, frame 개수는 2<sup>20</sup>까지라고 했기 때문
ex) 
•	페이지 크기: 100칸
•	논리 주소: 3번 페이지의 50번째
•	페이지 테이블: 페이지 3 → 프레임 7
•	물리 주소 = 700 + 50 = 750번지

![[Pasted image 20250522171843.png|200]]
#### Paging Hardware
![[Pasted image 20250522171911.png|300]]
1. CPU가 page number(p)를 이용하여 page table에서 frame을 가져온다
2. frame과 page offset(d)를 이용하여 physical address를 계산한다.
#### Example of Paging
![[Pasted image 20250522171923.png|300]]
## 내부 단편화 계산
• page size = 2048바이트
• process size = 72,766바이트
• 35 pages + 1,086 bytes
• 내부 단편화 = 2048 - 1086 = 962바이트
• 최악의 경우 단편화 = page size - 1byte
• 평균적으로 단편화 = ½ page size

• 작은 페이지 크기가 바람직한가?
	• 하지만 작은 페이지 크기는 페이지 테이블 항목 수를 증가시킴
		• 예: 32비트 주소 공간에 4KB 페이지 사용 시 페이지 테이블 크기 = 2<sup>(32−12)</sup> entries \* 20bit = 2.5MB
		2<sup>20</sup>개 page가 있고 각 page마다 할당된 frame 번호(20bit)
	• 페이지 크기는 시간이 지남에 따라 증가 추세 (4KB, 2MB, 1GB 등)
• 이제 프로세스 관점의 메모리와 실제 물리 메모리는 크게 다름
## Free Frames
  ![[Pasted image 20250522172822.png|400]]
## 페이지 테이블 구현
• 페이지 테이블은 메인 메모리에 유지
• *Page-table base register (PTBR)* 는 페이지 테이블 위치를 가리킴
• *Page-table length register (PTLR)* 는 페이지 테이블의 크기를 나타냄

• 이 방식에서는 데이터/명령어 접근마다 두 번의 메모리 접근이 필요함
(logical 주소를 physical 주소로 변환하기 위해서는 page table을 먼저 읽어와서 page table을 이용해서 physical address를 찾아야 한다.)
• 한 번은 페이지 테이블, 한 번은 실제 데이터/명령어
• 이 문제는 *Translation Look-Aside Buffer(TLB)* 라는 고속 하드웨어 캐시를 사용하여 해결
• 일부 TLB는 address-space identifiers(ASID)를 각 항목에 저장 (어떤 process 정보인지)
	• 프로세스를 고유하게 식별하여 주소 공간 보호 제공
	• 그렇지 않으면 컨텍스트 스위치마다 TLB를 비워야 함
• TLB는 일반적으로 작음 (64 ~ 1,024 entries)
• TLB 미스 발생 시, 해당 항목을 TLB에 적재해 이후 접근을 빠르게 함 (page table entry를 읽어옴으로서)
	• 교체 정책도 고려해야 함
## Paging Hardware with TLB
![[Pasted image 20250522174208.png|400]]
page table도 physical memory 어딘가에 있다
1. CPU가 page number(p)를 이용하여 TLB에서 frame 주소를 가져온다.
	1. TLB miss면, page table에서 frame 주소를 가져오고 주소를 TLB에 반영한다.
2. frame과 page offset(d)를 이용하여 physical address를 계산한다.

## 유효 접근 시간(EAT)
여기서는 캐시를 고려하지 않음
• ![[Pasted image 20250527170253.png|200]]
• 𝑡<sub>𝑇</sub>: TLB 접근 시간
• 𝑡<sub>𝑀</sub>: 메모리 접근 시간
• 𝛼: TLB 적중률 (0 ≤ 𝛼 ≤ 1)
TLB 적중시 TLB 접근, 메모리 접근(page table)
TLB MISS시 TLB 접근, 메모리 접근(page table)하고 추가적으로 메모리 접근
#### Example
• 𝛼 = 80%, 𝑡𝑇 = 5ns, 𝑡𝑀 = 100ns → EAT = 125ns
• 𝛼 = 99%, 𝑡𝑇 = 5ns, 𝑡𝑀 = 100ns → EAT = 106ns
• 𝛼 = 100%, 𝑡𝑇 = 5ns, 𝑡𝑀 = 100ns → EAT = 105ns
TLB hit 확률이 올라가면서 EAT가 점차 낮아진다
## 페이지 테이블 항목의 추가 비트 
• 각 프레임에 보호 비트를 부여하여 메모리 보호 구현
	• read-only 또는 read-write access 여부 표시
	• excute-only 등의 추가 비트를 지정할 수도 있음
• 페이지 테이블의 각 항목에는 valid/invalid bit 포함
	• valid(1): 해당 페이지가 프로세스의 논리 주소 공간 내에 있음
	• invalid(0): 해당 페이지가 프로세스의 논리 주소 공간에 없음
• 위반 시 커널로 트랩 발생
![[Pasted image 20250527171025.png|100]]
invalid 영역은 실제 physical address에 존재하지 않음
#### valid bit example
![[Pasted image 20250527171229.png|300]]
이거 말고도 page table에는 여러가지 부가 정보들이 붙는다.
6,7 영역은 값이 없으니까 invalid bit로 표기해둠
## 공유 페이지
• 공유 코드(또는 데이터)
	• read only(재진입 가능한) 코드의 하나의 복사본을 여러 프로세스가 공유
	• 다중 스레드가 같은 프로세스 공간을 공유하는 것과 유사
	• read-write 공유가 허용되면 inter-process communication에도 유용
• 개인 코드 및 데이터
	• 각 프로세스는 코드 및 데이터의 개별 복사본을 유지
	• 개인 코드와 데이터용 페이지는 논리 주소 공간 내 아무 위치에나 존재 가능
#### shared page example
![[Pasted image 20250527171349.png|300]]
라이브러리 코드같은 경우는 다들 읽기만 할것이므로 공유해서 사용할 수 있다
위 예시에서 ed들은 공유 페이지로 사용되고 있는 것
따라서 ed1,2,3는 메모리에 하나만 올라와 있다
## 페이지 테이블 크기
- 32비트 논리 주소 공간을 사용하는 현대 컴퓨터 가정
	- process가 사용하는 메모리 크기가 최대 4GB
	- ![[Pasted image 20250527172111.png|100]]
- 페이지 크기 4KB (2<sup>12</sup>)
- 페이지 테이블은 2<sup>20</sup> = 약 100만 개 항목
- 항목당 4바이트이면, 페이지 테이블당 4MB 필요
	- 이를 메인 메모리에 연속적으로 할당하고 싶지 않음
- 해결 방법:
	- 계층적 페이징(Hierarchical Paging)
	- 해시 기반 페이지 테이블(Hashed Page Table)
## 계층적 페이지 테이블
![[Pasted image 20250527171741.png|200]]
• 논리 주소 공간을 여러 페이지 테이블로 분할
• 간단한 방식은 2단계 페이지 테이블(two-level page table) 사용
outer page table: inner page table들이 어떤 frame에 저장되었는지
### Two level page table
![[Pasted image 20250527172934.png|300]]
