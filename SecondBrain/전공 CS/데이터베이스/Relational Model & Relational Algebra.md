- attributes: A<sub>1</sub>, A<sub>2</sub>, A<sub>3</sub>, ... A<sub>n</sub> column에 해당하는 개념
- domains: D<sub>1</sub>, D<sub>2</sub>, D<sub>3</sub>, ... D<sub>n</sub> attributes가 들어갈 수 있는 모든 값의 집합 ex) Integer
	- atomic: 단일한 값이어야 한다, 데이터를 사용하는 사람도 쪼개서 사용하면 안된다.
- relation: r ⊆ D<sub>1</sub> X D<sub>2</sub> X D<sub>3</sub>, ... D<sub>n</sub> 
	- 여러개의 tuple로 이루어져 있다.
- tuples: DB 테이블에서 row로 표현됨
## Keys
K ⊆ R
R = (A<sub>1</sub>, A<sub>2</sub>, A<sub>3</sub>, ... A<sub>n</sub>)
값을 대표할 수 있는 속성들의 subset
### super key
어떤 tuple을 identify 하는데 충분한 키
사실 R 자체도 super key다
### candidate key
super key 중 minimal한 key
ex) (Id, name)은 super key지만, id만으로 식별되지 않으므로 candidate key는 아니다.
ex) (Id), (학과번호, 학과별 수업 번호) 둘 다 나누게되면 식별불가능해지므로 둘 다 minimal하다고 볼 수 있다.
### primary key
candidate key중 선택된 key
### Foreign key
일종의 제약조건이다.

![[Pasted image 20250318133755.png|200]]
time_slot_id는 현재 제약조건을 가지지는 않는다.
## Relational Query Languages
관계형 질의 언어는 함수형, 명령형, 선언형 방식으로 나눌 수 있다,
- 순수 관계형 질의 언어
	- Relational Algebra (관계 대수)
		- procedural 언어로 표현되었으나, 함수형임
	- Tuple Relational Calculus (튜플 관계 해석학)
		- 선언형 언어
	- Domain Relational Calculus (도메인 관계 해석학)
		- 선언형 언어
- 관계 연산자
	- 함수형 관계형 질의 언어에서 제공하는 연산자
	- 단일 관계 또는 두 개의 관계에 적용
	- 연산 결과는 항상 하나의 관계
## Relational Algebra
### basic operations
앞글자따서 만듦
Select: 𝜎 (sigma)
Project: ∏ (Pi)
Union:  ∪
Set difference: –
Cartesian product: ×
Rename: ρ (rho)