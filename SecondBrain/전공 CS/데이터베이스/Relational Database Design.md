스키마를 합칠까?

instructor와 department 테이블을 합친다고 가정해보자.

big_instructor(ID, name, salary, dept_name, building, budget)

이 결과는 정보의 중복(repetition) 문제를 발생시킬 수 있다.

⸻

더 작은 스키마는 어떨까?

만약 우리가 처음부터 big_instructor 스키마를 갖고 있었다면, 그것을 instructor와 department로 분해(decompose) 해야 한다는 것을 어떻게 알 수 있을까?

하나의 규칙을 생각해보자:
“만약 (dept_name, building, budget)라는 스키마가 있다면, dept_name은 후보 키(candidate key) 여야 한다.”

이를 함수적 종속성(functional dependency) 으로 표현하면 다음과 같다:

dept_name → building, budget

하지만 big_instructor에서는 dept_name이 후보 키가 아니기 때문에, 같은 학과의 building과 budget 정보가 반복 저장되어야 할 수도 있다.
→ 이 사실은 big_instructor를 분해해야 함을 의미한다.

⸻

모든 분해가 올바른 것은 아니다

예를 들어, 다음 테이블을 고려해보자:

employee(ID, name, street, city, salary)

이것을 다음처럼 분해한다고 해보자:

employee1(ID, name)  
employee2(name, street, city, salary)

이 경우 Lossy Decomposition (정보 손실 분해) 가 발생한다.
→ 원래의 employee 릴레이션을 복원할 수 없다.
→ 따라서 이는 잘못된 분해이다.

⸻

제1정규형 (First Normal Form, 1NF)

도메인이 원자적(atomic) 이라는 것은 그 구성 요소들이 더 이상 나눌 수 없는 단위로 간주된다는 뜻이다.

비원자적 도메인(non-atomic domain) 의 예:
	•	이름들의 집합, 복합 속성
	•	CS101처럼 분할 가능한 식별자

릴레이션 스키마 R이 1NF에 속하려면, R의 모든 속성의 도메인이 원자적이어야 한다.

비원자적 값은 저장을 복잡하게 만들고 데이터 중복을 유발할 수 있다.
예시: 고객마다 계좌 목록, 계좌마다 소유자 목록을 저장하는 경우

일반적으로는 모든 릴레이션이 제1정규형을 만족한다고 가정한다.

⸻

원자성(Atomicity) 은 사실 도메인이 어떻게 사용되느냐에 달려 있다.
예: 문자열(string)은 보통은 원자적이라 여겨진다.
하지만 학생의 학번이 CS0012, EE1127 형식이라고 할 때,
앞의 두 문자를 추출해 학과 정보를 파악한다면 → 이는 원자적 도메인이 아니다.

이처럼 정보를 어플리케이션 코드에서 해석하게 되면,
데이터베이스 설계의 목적에 어긋난다. (정보가 인코딩된 형태로 저장됨)

⸻

목표 — 다음을 위한 이론 정립
	1.	릴레이션 R이 “좋은 형태(good form)”인지 판단
	2.	좋은 형태가 아닐 경우, R을 {R1, R2, ..., Rn}으로 분해
	•	각각은 좋은 형태여야 하며
	•	Lossless-Join 분해여야 함 (원래 릴레이션을 복원 가능)

이 이론은 함수적 종속성(functional dependency) 을 기반으로 한다.

⸻

함수적 종속성 (Functional Dependencies)
	•	릴레이션이 만족해야 하는 제약 조건 중 하나
	•	특정 속성 집합의 값이 다른 속성 집합의 값을 유일하게 결정해야 한다는 조건
	•	함수적 종속성은 Key(키) 개념을 일반화한 것이다.

⸻

필요하면 다음 부분도 이어서 도와줄게.