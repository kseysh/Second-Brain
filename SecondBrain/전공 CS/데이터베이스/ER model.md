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
#### example
proj_guide에서 instructor로 향하는 화살표는 학생마다 프로젝트에 대해 최대 한 명의 지도교수가 있음을 의미

• 만약 화살표가 두 개 이상이면 의미를 정의하는 방식은 두 가지가 있음:
예시: A, B, C 사이의 삼항 관계 R에서 B와 C로 향하는 화살표가 있을 경우
1. 각 A는 B와 C에서 유일한 조합과 연결된다
2. 각 (A, B) 쌍은 유일한 C와 연결되고, 각 (A, C) 쌍은 유일한 B와 연결된다
=> 다중 해석의 혼동을 방지하기 위해, 하나 이상의 화살표를 사용하는 것을 금지함