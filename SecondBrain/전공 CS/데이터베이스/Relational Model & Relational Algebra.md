- attributes: A<sub>1</sub>, A<sub>2</sub>, A<sub>3</sub>, ... A<sub>n</sub> column에 해당하는 개념 -> 대문자로 쓴다
- domains: D<sub>1</sub>, D<sub>2</sub>, D<sub>3</sub>, ... D<sub>n</sub> attributes가 들어갈 수 있는 모든 값의 집합 ex) Integer
	- atomic: 단일한 값이어야 한다, 데이터를 사용하는 사람도 쪼개서 사용하면 안된다.
- relation: r ⊆ D<sub>1</sub> X D<sub>2</sub> X D<sub>3</sub>, ... D<sub>n</sub> 
	- 여러개의 tuple로 이루어져 있다. ->  소문자로 쓴다
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
관계형 질의 언어는 Functional(함수형), Imperative(명령형), declarative(선언형) 방식으로 나눌 수 있다,
- 순수 관계형 질의 언어
	- Relational Algebra (관계 대수)
		- 함수형
	- Tuple Relational Calculus (튜플 관계 해석학)
		- 선언형 언어
	- Domain Relational Calculus (도메인 관계 해석학)
		- 선언형 언어
- 관계 연산자
	- 함수형 관계형 질의 언어에서 제공하는 연산자
	- 단일 관계 또는 두 개의 관계에 적용
	- 연산 결과는 항상 하나의 관계
# Relational Algebra
조건에 따라 Relational Algebra로 표현할 수 있도록 해야함
### basic operations
앞글자따서 만듦
Select: 𝜎 (sigma)
Project: ∏ (Pi)
Union:  ∪
Set difference: –
Cartesian product: ×
Rename: ρ (rho)
## Select 𝜎
selection of tuples
![[Pasted image 20250318135824.png|200]]
row를 구하는 것 (SQL 중 select보다는 where절에 가깝다)
## Project ∏
selection of columns
![[Pasted image 20250318140036.png|200]]
column을 구하는 것 (SQL 중 select에 가깝다)
set을 구하므로 중복이 있다면, 중복이 없는 것과 같다고 간주한다.
## Cartesian Product ×
SQL 중 FROM 절에 가깝다
![[Pasted image 20250318140842.png|200]]
## Natural Join ⋈
![[Pasted image 20250318142457.png|200]]
관계 R과 S의 natural join은 스키마 `R ∪ S`에 대한 관계이다.
Nautral Join은 Equi-Join(조건이 equal로만 이루어진 조인)이다
⋈ = ∏<sub>조건</sub>(𝜎<sub>조건</sub>(r X s))
- 각각 r에서의 tuple t<sub>r</sub>과 s에서의 튜플 t<sub>s</sub>의 쌍을 고려한다
- 만약 t<sub>r</sub>과 t<sub>s</sub>가 `R ∩ S`에 속하는 모든 속성에서 동일한 값을 가진다면 결과 집합에 튜플 t를 추가한다.
	- t는 t<sub>r</sub>이 r에서 가진 값과 동일한 값을 가진다
	- t는 t<sub>s</sub>가 s에서 가진 값과 동일한 값을 가진다.
Natural Join은 이름이 같으면 그냥 연결함
그래서 ∏<sub>name, title</sub>(instuctor ⋈ teaches ⋈ Name)
를 할 때, dept_name도 같이 연결되어 버릴 수 있다.
=> 따라서 side effect를 주의하고 세타 조인을 사용해볼 수도 있다.
& 다시 한 번 보기

Natural join 은 associative하고
((instuctor ⋈ teaches) ⋈ Name) =(instuctor ⋈ (teaches ⋈ Name))
Natural join 은 commutative하다.
(instuctor ⋈ teaches) = (teaches ⋈ instructor)
### Theta Join ⋈<sub>θ</sub>
두 개의 릴레이션을 특정 조건(θ, 비교 연산 포함)을 사용하여 결합하는 연산이다. 
일반적인 등가 조인(equi-join)과 달리, 등호(=)뿐만 아니라 <, >, <=, >=, != 등의 연산자를 사용할 수 있다.
θ-조인은 R × S(카티션 곱) 후에 조건(θ)을 적용하여 필터링하는 방식

Natural Join은 컬럼명이 같은 엔티티끼리만 조인하는 것이지만, 컬럼명이 다르다면 Theta Join을 한다.
![[Pasted image 20250318142719.png|300]]

Theta Join 자체에는 Projection이 없다
단순히 필터링을 수행할 뿐 원하는 속성을 선택하는 역할은 하지 않는다.
## Union ∪
![[Pasted image 20250320134414.png|200]]
중복은 제거된다.
## difference -
![[Pasted image 20250320134444.png|200]]
중복은 제거된다
## Rename ρ
관계 대수 표현식의 결과에 이름을 붙일 수 있게 해주어 그 결과를 참조할 수 있게 한다.
ex) ρ(E) : 표현식 E에 이름 x를 붙여 반환한다.
#### example
ID가 12121인 교수보다 더 많은 급여를 받는 교수들의 ID 찾기

![[Pasted image 20250405230525.png]]

$$
\pi_{i.ID} \left( \rho_i(\text{instructor}) \Join_{i.salary > j.salary \wedge j.ID = 12121} \rho_j(\text{instructor}) \right)
$$