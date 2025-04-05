###### 파일 시스템의 문제
- 데이터 중복성과 불일치 (정규화)
- 원자성 보장 x (Atomic)
- 무결성 보장 x (Consistency)
- 데이터의 고립 및 분리 (Isolation)
- 동시성으로 인한 데이터 불일치 (Isolation)
- 접근 제어의 어려움
- 데이터 접근의 어려움
###### DB 세 가지 layer 추상화
- physical level
	- 실제 데이터가 저장되는 곳
- logical level
	- 어떤 속성이 있는가
	- 어떤 데이터를 가지고 있어야 하는가
- view level
	- 일반 사용자들이 쉽게 이해할 수 있는 개념들로 이뤄진 레벨
	- logical level의 부분집합일 수도 있고
	- view level이 새로 생성될 수도 있음 (다대다 만들 때 만드는 관계같은 것)
###### Physical Data Independence
- 물리적 스키마를 변경해도 논리적 스키마를 변경하지 않을 수 있는 능력
- DB를 두 대로 변경하던, 여러 파일로 저장하던 table은 변하지 않는다.
###### 스키마의 뜻과 스키마의 종류
- 데이터 베이스의 논리적 구조
- physical schema
	- physical level에서의 schema
- logical schema
	- logical level에서의 schema
###### Instance
특정 시점에서의 데이터베이스 실제 내용
###### Super key
어떤 tuple을 identify 하는데 충분한 키
사실 R 자체도 super key다
###### candidate key
super key 중 minimal한 key
ex) (Id), (학과번호, 학과별 수업 번호) 둘 다 나누게되면 식별불가능해지므로 둘 다 minimal하다고 볼 수 있다.
###### PK
candidate key 중 선택된 key
###### 제약 조건을 가질 때 / 가지지 않을 때 표기
![[Pasted image 20250318133755.png|200]]
화살표를 쓰냐(제약조건을 가짐) 안쓰냐(논리적으로만 연결됨)
###### SQL은 무슨 방식인지?
Relation Algebra로, Functional 방식
###### select tuples with A=B and D > 5를 Relational Algebra로
![[Pasted image 20250405224447.png]]
###### selecting instructor tuples with salary greater than 85000을 Relational Algebra로
𝜎<sub>salary>=85000</sub>(instuctor)
###### Select A and C를 Relational Algebra로
![[Pasted image 20250405224626.png]]
###### Selecting attributes ID and salary from the instructor relatijon Relational Algebra로
∏<sub>ID,salary</sub>(instructor)
###### Natural Join을 다르게 표현하면
⋈ = ∏<sub>조건</sub>(𝜎<sub>조건</sub>(r X s))
###### Theta Join을 다르게 표현
![[Pasted image 20250318142719.png|300]]
###### Theta join 특징
Theta join자체에는 Projection(select)를 진행하지 않음
###### Union 특징
중복 제거
###### ID가 12121인 교수보다 더 많은 급여를 받는 교수들의 ID 찾기
![[Pasted image 20250405230525.png]]
rho는 바깥으로 해도 됨
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
