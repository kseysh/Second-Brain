## 개요 (Overview)
좋은 관계형 설계(Relational Design)의 특징
Atomic Domains와 제1정규형(First Normal Form)
함수적 종속성(Functional Dependencies)을 활용한 분해(Decomposition)
함수적 종속성 이론(Functional Dependency Theory)
## Combine Schemas?
instructor 테이블과 department 테이블을 결합한다고 가정하자:
big_instructor(ID, name, salary, dept_name, building, budget)
이 결과는 정보의 중복을 초래할 수 있다.
![[Pasted image 20250415140149.png|400]]
여기서 Kim을 삭제하는 순간, Elec. Eng dept는 사라지게 된다.
## What About Smaller Schemas?
처음부터 big_instructor라는 스키마가 있었다고 가정하자. 이를 instructor와 department로 나눠야 한다는 것을 어떻게 알 수 있을까?
규칙을 다음과 같이 작성할 수 있다:
“만약 (dept_name, building, budget)이라는 스키마가 있다면, dept_name은 후보 키(candidate key)이다.”
이것을 함수적 종속성으로 나타내면:
dept_name → building, budget
그런데 big_instructor에서는 dept_name이 후보 키가 아니기 때문에, 같은 학과의 building과 budget 정보가 반복될 수 있다.
이는 big_instructor를 분해할 필요가 있다는 것을 시사한다.
하지만 모든 분해가 올바른 것은 아니다.
## 정보 손실 분해 (Lossy Decomposition)
employee(ID, name, street, city, salary)를
employee1(ID, name)과 employee2(name, street, city, salary)로 분해한다고 가정해보자.
이렇게 분해하면 Join시에 정보를 잃게 된다. 즉, 원래의 employee 관계를 복원할 수 없다.
따라서 이는 lossy decomposition (정보 손실이 있는 분해)이다.
![[Pasted image 20250415135902.png|300]]
이것의 문제: 오른쪽에 key가 없음
## Lossless join decomposition
![[Pasted image 20250415135951.png|300]]
equi-join을 했을 때, 원래 정보를 이것과 같이 그대로 가져올 수 있어야 lossless join decomposition이다.

## 제1정규형 (First Normal Form)
- 도메인이 atomic하다는 것은, 해당 도메인의 요소가 더 이상 나눌 수 없는 단위라는 의미이다.
	- Atomic하지 않은 도메인의 예시:
		- 이름 집합(Set of names), 복합 속성(Composite attributes)
		- CS101과 같이 분해 가능한 학번(ID) 형식
- 어떤 관계 스키마 R이 제1정규형에 있다는 것은, R의 모든 속성(attribute)의 도메인이 atomic하다는 뜻이다.
- Atomic하지 않은 값은 저장을 복잡하게 만들고, 데이터 중복을 유발한다.
	- 예: 고객마다 여러 계좌가 저장되고, 각 계좌마다 여러 소유자가 저장되는 경우.
	- 우리는 기본적으로 모든 관계가 제1정규형에 있다고 가정한다.
- Atomicity는 도메인 사용 방식에 따라 결정된다
	- 예: 문자열(String)은 일반적으로 나눌 수 없는 것으로 간주된다.
	- 하지만 학생들에게 CS0012나 EE1127과 같은 형식의 학번을 부여하고,
	- 이 중 앞의 두 글자를 추출해 학과를 알아낸다면, 이 도메인은 atomic하지 않게 된다.
	- 이런 방식은 application 프로그램에서 정보를 해석하게 되는 구조로, 데이터베이스의 역할을 침해하게 되므로 바람직하지 않다.
## 목표 — 다음을 위한 이론 정립
어떤 관계 R이 “좋은” 형태인지 판단하고,
그렇지 않다면 R을 {R1, R2, …, Rn}으로 분해하되,
	•	각 관계가 좋은 형태가 되도록 하고
	•	Lossless-join decomposition (정보 손실이 없는 분해)이 되도록 한다.
이 이론의 기반은 **함수적 종속성(Functional Dependencies)** 이다.
## 함수적 종속성 (Functional Dependencies)
•	가능한 관계 집합에 대해 제약 조건을 설정한다.
•	어떤 속성 집합의 값이 다른 속성 집합의 값을 유일하게 결정해야 한다.
•	함수적 종속성은 **키(key)** 의 개념을 일반화한 것이다. 
#### example 1
- R을 릴레이션 스키마라고 하자
- ![[Pasted image 20250416230834.png|100]]
- 함수적 종속성: α → β
- α → β 는 릴레이션 r(R)에 대해 다음 조건을 만족할 때 성립한다:
- 임의의 두 튜플 t<sub>1</sub>, t<sub>2</sub>가 속성 α에서 같은 값을 가지면, 속성 β에서도 같은 값을 가진다.
- 즉, ![[Pasted image 20250416231015.png|150]]
#### example 2
릴레이션 r(A, B)
```sql
A  B
1  4
1  5
3  7
```
이 인스턴스에서 A → B는 성립하지 않지만, B → A는 성립한다.
(1이 4,5를 둘 다 결정하면 안되므로)
## Functional Dependencies
•	K는 릴레이션 스키마 R에 대해 슈퍼키이다 ⇔ K → R (key는 다른 attribute를 모두 결정해야 함)
•	K는 R의 후보 키이다 ⇔	K → R 이고, a ⊂  K에 대해 a → R가 성립하지 않음 
•	함수적 종속성은 슈퍼키만으로는 표현할 수 없는 제약 조건들을 표현할 수 있게 해준다.
#### example
big_instructor(ID, name, salary, dept_name, building, budget)
다음과 같은 함수적 종속성은 성립할 것으로 기대한다:
	•	dept_name → building
	•	ID → building
그러나 아래와 같은 종속성은 기대하지 않는다:
	•	dept_name → salary
## Use of Functional Dependencies
함수적 종속성은 다음과 같은 목적을 위해 사용된다:
•	주어진 함수적 종속성 집합에 대해 릴레이션이 합법적인지를 검사하기 위해 사용합니다.
	•	어떤 릴레이션 r이 함수적 종속성 집합 F에 대해 합법적이면, 우리는 r이 F를 만족한다고 말합니다.
•	합법적인 릴레이션들의 집합에 제약 조건을 명시하기 위해 사용합니다.
	•	어떤 릴레이션 스키마 R에 대해 모든 합법적인 릴레이션이 함수적 종속성 집합 F를 만족하면, 우리는 F가 R에서 성립한다고 말합니다.

주의: 릴레이션 스키마의 특정 인스턴스는 어떤 함수적 종속성을 만족할 수는 있지만, 그 함수적 종속성이 모든 합법적인 인스턴스에서 성립하는 것은 아닙니다.
예를 들어, instructor 릴레이션의 특정 인스턴스가 **우연히** name → ID를 만족할 수 있습니다.
## Functional Dependencies
•	어떤 함수적 종속성이 모든 릴레이션 인스턴스에서 만족된다면, 그 종속성은 자명(trivial) 하다고 한다.
•	일반적으로, α → β가 자명하려면 β ⊆ α여야 한다.
#### example
•	ID, name → ID
•	name → name
## Closure of Functional Dependencies
•	함수적 종속성 집합 F가 주어졌을 때, F로부터 논리적으로 도출될 수 있는 다른 함수적 종속성들이 존재할 수 있다.
•	F로부터 논리적으로 도출 가능한 모든 함수적 종속성들의 집합을 F의 closure 라고 한다.
•	우리는 F의 Closure를 F⁺로 표기한다.
•	F⁺는 F의 상위 집합(superset) 이다.
#### example
A → B이고 B → C라면, 우리는 A → C를 추론할 수 있다.
## [[BCNF 정규형]], (Boyce-Codd Normal Form)
릴레이션 스키마 R이 함수적 종속성 집합 F에 대해 BCNF에 속한다는 것은, **F⁺에 속하는 모든 함수적 종속성 α → β에 대해** 다음 조건 중 하나 이상을 만족하는 경우를 말한다:
•	α ⊆ R, β ⊆ R이고,
1.	α → β가 자명하다 (즉, β ⊆ α)
2.	α가 R에 대한 슈퍼키이다
즉, *결정은 Key만 할 수 있다*
#### BCNF에 속하지 않는 예시 스키마:
big_instructor (ID, name, salary, dept_name, building, budget)
이 스키마는 dept_name → building, budget이라는 함수적 종속성이 존재하지만,
dept_name이 슈퍼키가 아니기 때문에 BCNF를 만족하지 않는다.
## Decomposing a Schema into BCNF
•	어떤 스키마 R이 있고, α → β라는 비자명(non-trivial) 한 함수적 종속성이 BCNF를 위반한다고 가정하자.
•	이 경우, 스키마 R을 다음과 같이 분해한다:
•	(α ∪ β)
•	(R − (β − α))
#### example
•	α = dept_name
•	β = building, budget
•	따라서 big_instructor는 다음 두 릴레이션으로 대체된다:
•	(α ∪ β) = (dept_name, building, budget)
•	(R − (β − α)) = (ID, name, salary, dept_name)
## BCNF와 종속성 보존 (Dependency Preservation)
•	함수적 종속성을 포함한 제약 조건들은, 하나의 릴레이션에만 관련될 때를 제외하면 실제로 검사하는 데 비용이 많이 들 수 있다.
•	만약 어떤 분해에서 각 릴레이션에 대해 개별적으로 종속성을 검사하는 것만으로도 전체 종속성들이 유지됨을 보장할 수 있다면, 그 분해는 종속성 보존(dependency preserving) 이라고 한다.
•	그러나 *BCNF와 종속성 보존을 동시에 만족시키는 것이 항상 가능한 것은 아니므로*, 우리는 더 약한 정규형인 제3정규형(3NF) 을 고려한다.
## [[제 3 정규형]] (Third Normal Form, 3NF)
릴레이션 스키마 R이 제3정규형에 속하려면,
F⁺에 속하는 모든 함수적 종속성 α → β에 대해 다음 조건 중 하나 이상을 만족해야 한다:
	1.	α → β가 자명하다 (즉, β ⊆ α)
	2.	α가 R의 super key이다
	3.	**β − α에 속한 각 속성 A**는 R의 어떤candidate key에 포함되어야 한다
(주의: 각각의 속성은 서로 다른 candidate key에 속해도 무방하다)

•	릴레이션이 BCNF에 속하면, 위의 첫 두 조건 중 하나는 반드시 만족하므로 3NF에도 속하게 된다.
•	세 번째 조건은 종속성 보존을 보장하기 위해 BCNF를 최소한으로 완화한 것이다
### BCNF vs. 3NF
![[Pasted image 20250417140802.png|300]]
advisor(<u>s_ID</u>, i_ID, <u>dept_name</u>)
다음과 같은 추가 제약 조건이 있다고 가정하자: `i_ID → dept_name`
f<sub>1</sub>: s_ID, dept_name → i_ID
f<sub>2</sub>: i_ID → dept_name

#### 분해가 필요한가?
- BCNF
dept_name을 결정하는 i_ID는 super key가 아니어서 BCNF 위배 (<u>i_ID</u>, dept_name) (<u>s_ID</u>, <u>i_ID</u>) 로 나눠야함
=> 이는 Dependency가 Preservation되지 않음 (독립적으로 f<sub>1</sub>을 만족시킬 수 없음, join을 해야 f<sub>1</sub>이 만족하는지 확인할 수 있음)
- 3NF
dept_name이 candidate key에 속해있다면 3NF만족 (이때는 Dependency Preservation을 만족함)

중복을 허용하지 않고 Dependency Preservation을 할 것인지, 약간의 중복을 허용하여 Dependency Preservation을 위배할 것인지
## 정규화의 목적
•	관계 스키마 R와 함수적 종속성 집합 F가 주어졌을 때:
•	관계 스키마 R이 “좋은 형태(good form)”인지 판단한다.
•	만약 R이 좋은 형태가 아니라면, 다음 조건을 만족하도록 R을 \{R<sub>1</sub>, R<sub>2</sub>, …, R<sub>n</sub>\}으로 분해한다:
	•	각 관계 스키마가 좋은 형태를 만족해야 한다.
	•	분해는 Lossless-Join Decomposition 이어야 한다. (BCNF도 Lossless-Join Decomposition을 지킴)
	•	가능하다면, 종속성 보존(Dependency Preserving) 되는 분해가 바람직하다.