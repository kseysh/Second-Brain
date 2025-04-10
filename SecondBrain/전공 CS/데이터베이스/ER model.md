## Entity
다른 객체들과 구별 가능한 객체
attrubutes를 가진다.
## Entity set
동일한 유형의 엔티티들의 모음으로 같은 속성을 공유함
## Domain
각 속성이 가질 수 있는 허용된 값들의 집합
## Attrubute Types
- Simple and composite attributes.
- Single-valued and multivalued attributes
- Derived attributes
	- ex) 생년월일로부터 계산된 나이
## Composite Attributes
![[Pasted image 20250408141008.png|300]]
## Relationship Sets
2개 이상의 엔티티 집합에서 가져온 n개의 엔티티간 조합
#### example
![[Pasted image 20250408141301.png|300]]
(44553,22222) ∈  advisor
### Relationship Set advisor
![[Pasted image 20250408141417.png|300]]
## ERD
![[Pasted image 20250408141435.png|300]]
• 사각형(Rectangle): 엔티티 집합
• 마름모(Diamond): 관계 집합
• 속성은 엔티티 사각형 안에 나열
• 밑줄은 PK를 나타냄
![[Pasted image 20250408141648.png|300]]
관계는 설명 속성도 가질 수 있음
ex) created_at을 나타내는 date 속성을 가질 수 있음

관계는 관계에 참여하는 엔티티들로만 식별된다. (FK가 두 엔티티가 아닌 외부에 있으면 안됨)
## Degree of a Relationship Set
### Binary Relationship
두 개의 엔티티 집합을 포함 (degree 2)
대부분의 DB 시스템에서 관계는 Binary Relationship임
### Ternary Relationship (삼항 관계)
#### example
학생들이 연구 프로젝트에 대해 교수의 지도 하에 작업함
![[Pasted image 20250408141920.png|300]]
proj_guide라는 관계는 instructor, student, project 사이의 Ternary relationship
## Roles
- 관계 내의 엔티티 집합들이 반드시 서로 달라야 할 필요는 없음
	- 같은 엔티티 집합이 관계 내에서 여러 역할을 할 수 있음
#### example
선수과목과 선수과목을 수강해야 들을 수 있는 과목은 모두 course를 참조한다
“course_id”와 “prereq_id”는 같은 entity set(course)을 가리키지만 역할(role)이 다름
![[Pasted image 20250408142143.png|300]]
## Mapping Cardinality Constraints
관계를 통해 하나의 엔티티가 다른 엔티티와 연결될 수 있는 수를 표현함
- 이진 관계에서 특히 중요
가능한 관계 유형
- One to one
- One to many
- Many to one
- Many to many
![[Pasted image 20250408142402.png|300]]
![[Pasted image 20250408142412.png|300]]
## Cardinality Constraints
카디널리티 제약은 선으로 표현:
• →: 하나(one)
• —: 여러 개(many)
## Relationship
one to one
• 교수는 advisor 관계를 통해 최대 한 명의 학생과만 연결될 수 있음
• 학생도 advisor 관계를 통해 최대 한 명의 교수와만 연결될 수 있음
![[Pasted image 20250408142853.png|300]]
one to many
•	교수는 advisor를 통해 여러 명(0명 포함)의 학생과 연결될 수 있음
•	학생은 advisor를 통해 최대 한 명의 교수와만 연결될 수 있음
![[Pasted image 20250408142911.png|300]]
many to one
•	교수는 advisor를 통해 최대 한 명의 학생과만 연결될 수 있음
•	학생은 advisor를 통해 여러 명(0명 포함)의 교수와 연결될 수 있음
![[Pasted image 20250408142922.png|300]]
many to many
•	교수는 advisor를 통해 여러 명(0명 포함)의 학생과 연결될 수 있음
•	학생도 advisor를 통해 여러 명(0명 포함)의 교수와 연결될 수 있음
![[Pasted image 20250408142933.png|300]]
## Cardinality Constraints on Ternary Relationship
삼항 또는 그 이상 차수를 가진 관계에서는 하나의 방향 화살표만 허용하여 cardinality contraints를 표시함
다시 써보면서 확인해보자
#### example
proj_guide에서 instructor로 향하는 화살표는 학생마다 프로젝트에 대해 최대 한 명의 지도교수가 있음을 의미

![[Pasted image 20250408144021.png|200]]  ![[Pasted image 20250408144508.png|200]]
• 만약 화살표가 두 개 이상이면 의미를 정의하는 방식은 두 가지가 있음:
예시: A, B, C 사이의 삼항 관계 R에서 B와 C로 향하는 화살표가 있을 경우
1. 각 A는 B와 C에서 유일한 조합과 연결된다
2. 각 (A, B) 쌍은 유일한 C와 연결되고, 각 (A, C) 쌍은 유일한 B와 연결된다
=> 다중 해석의 혼동을 방지하기 위해, 하나 이상의 화살표를 사용하는 것을 금지함
![[Pasted image 20250408144347.png|200]]
## 엔티티 집합의 관계 참여
### Total Participation
이중 선으로 표시되며, 엔티티 집합의 모든 엔티티가 관계 집합에 최소한 하나는 참여해야 함
#### example
![[Pasted image 20250408150400.png|250]]
section은 sec_course에 전체 참여함 → 모든 섹션은 반드시 하나의 강의(course)와 연결되어야 함
### Partial Participation
일부 엔티티는 어떤 관계에도 참여하지 않을 수 있음

ex) instructor의 advisor 참여는 partial participation -> 어떤 교수는 학생과 연결되지 않을 수 있음
## Redundant Attributes (중복 속성)
instructor 엔티티에 dept_name 속성이 있을 때, instructor와 department를 연결하는 inst_dept 관계가 있다면, dept_name 속성은 중복됨
이는 관계를 통해 이미 제공되는 정보를 속성으로 반복한 것이며 instructor에서 제거되어야 함
테이블로 변환할 때는, 어떤 경우엔 이 속성이 다시 포함되기도 함
## Keys
- super key
	- 엔티티를 고유하게 식별할 수 있는 하나 이상의 속성 집합
- Candidate key
	- super key에서 더 이상 속성을 뺄 수 없는 unique한 key
## 관계 집합의 key
- 관계 집합의 super key는 관련된 엔티티 집합의 PK 조합 (ex) advisor 관계에서는 (s_id, i_id)가 super key)
- 이 말은, 동일한 엔티티 집합 쌍은 특정 관계 집합에서 최대 하나의 관계만 가질 수 있다는 뜻
- 관계 집합의 PK는 관계의 cardinality에 따라 달라짐
• 일대일: 두 엔터티 중 어느 한 쪽의 PK
• 다대일: “다”에 해당하는 엔터티의 PK
• 다대다: 두 엔터티의 PK를 모두 합친 것
- candidate key가 여러 개일 경우, 관계의 의미를 고려해 PK를 선택해야 함
## Weak Entity sets
값 타입할 때 만들었던 pk 없는 엔티티 말하는듯
- PK가 없는 엔티티 집합
- 약한 엔티티 집합은 identifying (owner) entity set의 존재에 의존한다.
- identifying entity set과는 전체(total)이고, identifying entity에서 약한 entity로의 일대다(one-to-many) 관계를 가져야 한다.
- 이러한 identifying 관계는 이중 다이아몬드(double diamond)로 표시된다.
- 약한 엔티티 집합의 discriminator (또는 partial key)는 약한 엔티티 집합 내의 개별 엔티티들을 구분하는 속성들의 집합이다.
- 약한 엔티티 집합의 primary key는 의존하고 있는 강한 엔티티 집합의 primary key와 약한 엔티티 집합의 discriminator로 구성된다.
- 약한 엔티티 집합의 discriminator는 점선 밑줄(dashed underline)로 표시한다.
- 약한 엔티티와 연결된 identifying 관계는 이중 다이아몬드로 표현한다
#### example
section의 pk: (course_id, sec_id, semester, year)
![[Pasted image 20250408152813.png|300]]
주의: strong entity의 pk는 명시적으로 저장되지 않아도, 관계를 통해 암시적으로 연결됨
만약, course_id를 명시적으로 저장한다면, section은 강한 entity가 될 수 있지만, 이 경우 course와 section 사이의 관계가 속성에 의해 중복 정의됨
### ERD for a university Enterprise
![[Pasted image 20250408153007.png|400]]
## 관계 스키마로의 변환
•	엔티티 집합과 관계 집합은 데이터베이스의 내용을 표현하는 relation schemas로 변환될 수 있다.
•	E-R 다이어그램을 따르는 데이터베이스는 여러 스키마로 표현할 수 있다.
•	각각의 엔티티 및 관계 집합마다 고유한 스키마가 존재하며, 그 이름은 해당 엔티티 또는 관계 집합의 이름을 따른다.
•	각 스키마는 속성(컬럼)들을 가지며, 이들의 이름은 유일해야 한다.
## 단순 속성을 가진 엔티티 집합 표현
•	강한 엔티티 집합은 해당 속성들로 이루어진 스키마로 변환된다.
ex) `student(ID, name, tot_cred)`

•	약한 엔티티 집합은 identifying 강한 엔티티 집합의 primary key를 포함하는 컬럼을 추가하여 테이블로 변환된다.
ex) `section(course_id, sec_id, sem, year)`
![[Pasted image 20250410135525.png|300]]
## 복합 속성
•	복합 속성은 각 구성 요소로 분리하여 속성으로 표현된다.
예: name이 first_name과 last_name으로 구성된 경우 → name_first_name, name_last_name
•	단, 애매하지 않은 경우 prefix는 생략 가능하다.
## 다중값 속성
다중값 속성 M은 별도의 스키마 EM으로 표현된다.
EM은 엔티티 E의 primary key와 다중값 속성 M에 해당하는 속성으로 구성된다.
#### example
instructor의 다중값 속성 phone_number는 다음과 같은 스키마로 표현됨:
instructor_phone(ID, phone_number)
→ instructor가 전화번호 2개(456-7890, 123-4567)를 가질 경우:
(22222, 456-7890)
(22222, 123-4567)

특수한 경우: time_slot 엔티티는 primary key 외의 유일한 속성이 multivalued 속성일 때
→ 최적화: time_slot 엔티티에 해당하는 relation을 따로 만들지 않고, 다중값 속성에 해당하는 relation만 생성
→ 예: time_slot(<u>time_slot_id</u>, <u>day</u>, <u>start_time</u>, end_time)
⚠️ 이 경우 section의 time_slot 속성은 외래 키(foreign key)로 사용할 수 없음
![[Pasted image 20250410135931.png|300]]
time_slot: 