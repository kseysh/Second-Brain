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
이렇게 분해하면 정보를 잃게 된다. 즉, 원래의 employee 관계를 복원할 수 없다.
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
-	Atomicity는 도메인 사용 방식에 따라 결정된다
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


