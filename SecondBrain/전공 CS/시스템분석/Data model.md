use case 4번 기말
actor, preconditoin normal course에 관련 data, input data, source, postcondition
use case

ERD


## 핵심 정의
### 데이터 모델
•	비즈니스 시스템이 사용하는 데이터와 생성되는 데이터를 형식적으로 표현하는 방법
•	데이터가 수집되는 사람, 장소, 사물 및 그들 간의 관계를 보여줌
•	논리 데이터 모델은 데이터가 어떻게 저장, 생성, 조작되는지를 나타내지 않고 데이터의 조직만 보여줌
•	물리 데이터 모델은 실제로 데이터가 데이터베이스나 파일에 어떻게 저장되는지를 보여줌
•	데이터 모델은 프로세스 모델과 균형을 이루어야 함
### ERD (엔터티-관계 다이어그램)
•	데이터 모델을 묘사하는 데 널리 사용되는 방법
### 정규화
분석가가 데이터 모델을 검증할 때 사용하는 과정
### 데이터 모델링이 중요한 이유
•	데이터는 가능한 많은 프로세스가 공유할 수 있는 자원
•	데이터 조직은 예기치 못한 비즈니스 요구사항에 유연하고 적응 가능해야 함 → 이것이 데이터 모델링의 목적
### 기타 데이터 모델링 이슈
•	데이터 구조와 속성은 상대적으로 영구적이며, 데이터를 사용하는 프로세스보다 더 안정적
•	기존 시스템과 매우 유사한 경우가 많음
•	데이터 모델은 프로세스 모델보다 훨씬 작고 빠르게 구성됨
•	데이터 모델을 작성하는 과정에서 분석가와 사용자 간의 비즈니스 용어 및 규칙에 대한 합의가 빠르게 이루어질 수 있음
# 엔터티-관계 다이어그램
What do ERDs Tell us?
![[Pasted image 20250521182714.png|300]]
![[Pasted image 20250521182724.png|300]]
### ERD로 비즈니스 규칙을 나타내기
•	비즈니스 규칙은 시스템이 작동 중일 때 따라야 하는 제약 조건
•	ERD 기호는 어떤 엔터티 인스턴스가 존재해야만 다른 인스턴스가 존재할 수 있음을 표현 가능
	•	고객 인스턴스가 존재해야 해당 고객의 주문 인스턴스를 생성할 수 있음
	•	드론 부품 인스턴스가 존재해야 그 부품에 대한 주문 인스턴스를 생성할 수 있음
•	ERD 기호는 한 엔터티 인스턴스가 다른 엔터티 인스턴스에 하나만 연결될 수 있는지, 여러 개에 연결될 수 있는지를 표현
	•	하나의 고객 인스턴스는 여러 개의 주문을 생성할 수 있음, 각 주문은 오직 하나의 고객이 생성
	•	하나의 드론은 여러 화학 물질 요청에 포함될 수 있음, 각 요청은 하나의 화학 물질에만 해당
•	ERD 기호는 관련 엔터티 인스턴스가 없어도 특정 인스턴스가 존재할 수 있는지를 표현
	•	고객 인스턴스는 어떤 주문에도 포함되지 않고 존재할 수 있음
### ERD Example
![[Pasted image 20250521182813.png|300]]
### 엔터티(Entity)
•	데이터가 수집되는 사람, 장소, 사건 또는 사물
•	여러 번 발생하는 경우에만 엔터티로 간주됨
### CASE Entry for Entity
![[Pasted image 20250521182901.png|300]]
### 속성(Attributes)
•	엔터티에 대해 수집되는 정보
•	조직에서 실제로 사용하는 정보만 모델에 포함
•	속성 이름은 명사
•	명확성을 위해 속성 이름 앞에 엔터티 이름을 붙이기도 함
![[Pasted image 20250521183010.png|300]]
### CASE Entry for Attribute
![[Pasted image 20250521182941.png|300]]
### 식별자(Identifier) 유형
•	하나 이상의 속성이 엔터티 식별자로 작용할 수 있음 (각 인스턴스를 고유하게 식별)
•	여러 속성으로 구성된 경우 연결 식별자(Concatenated Identifier)라 부름
•	ID 번호를 만드는 등 인공 식별자(Artificial Identifier)도 가능
•	최종 식별자 결정은 설계 단계로 미룰 수 있음
![[Pasted image 20250521183034.png|300]]
### 관계(Relationships)
•	엔터티 간의 연관성
•	관계의 첫 번째 엔터티는 부모 엔터티, 두 번째는 자식 엔터티
•	관계 이름은 능동태 동사 형태여야 함
•	관계는 양방향으로 적용됨
### CASE Entry for Relationship
![[Pasted image 20250521183122.png|300]]
### Binary Relationships
![[Pasted image 20250521183148.png|300]]
동그라미: 선택적 관계
### 카디널리티(Cardinality)
•	한 엔터티의 인스턴스가 다른 엔터티의 인스턴스와 얼마나 자주 연관될 수 있는지를 나타냄
•	1:1 관계: 하나의 인스턴스는 다른 하나의 인스턴스와만 연관
•	1:N 관계: 하나의 인스턴스는 여러 인스턴스와 연관
•	M:N 관계: 여러 인스턴스들이 서로 여러 인스턴스와 연관
### 모달리티(Modality)
•	자식 엔터티의 인스턴스가 부모 엔터티 인스턴스 없이 존재할 수 있는지를 나타냄
•	Not Null: 자식 인스턴스가 유효하려면 부모 인스턴스가 반드시 존재해야 함
•	Null: 자식 인스턴스는 부모 인스턴스 없이도 유효할 수 있음
### 외래 키(Foreign Keys)
•	관계는 한 엔터티의 인스턴스가 다른 엔터티의 인스턴스와 연관됨을 의미
•	한 엔터티의 기본 키(primary key)가 다른 엔터티로 복사되어 외래 키로 사용됨
•	외래 키는 항상 자식 엔터티에 위치하며, 부모 엔터티의 기본 키와 반드시 일치해야 함
![[Pasted image 20250521183202.png|300]]
## ERD 작성
ERD가 어떻게 개발되는가
## 개요
•	ERD를 그리는 과정은 반복적인 시도와 수정의 과정임
•	ERD는 매우 복잡해질 수 있음
•	ERD 작성 단계
	•	엔터티 식별
	•	각 엔터티에 적절한 속성 추가
	•	관련 엔터티들을 연결하는 관계 도식화
## 엔터티 식별
•	주요 정보 범주를 식별
	•	가능한 경우, 프로세스 모델에서 데이터 저장소, 외부 엔터티, 데이터 흐름 등을 확인
	•	유스케이스의 주요 입력과 출력을 확인
•	시스템에서 해당 엔터티 인스턴스가 두 개 이상 발생하는지 확인
## 적절한 속성 추가
•	개발 중인 시스템과 관련 있는 속성을 식별
	•	프로세스 모델 저장소에서 데이터 흐름 및 저장소에 대한 세부사항 확인
	•	요구사항 정의서의 데이터 요구사항 확인
	•	잘 아는 사용자와 인터뷰
	•	기존 양식과 보고서에 대한 문서 분석 수행
•	엔터티의 후보 식별자 선택 (최종 결정은 설계 단계까지 미룰 수 있음)
## 관계 도식화
•	한 엔터티부터 시작하여, 관계를 맺는 모든 엔터티를 식별
•	적절한 동사구로 관계 설명
•	숙련된 사용자와 비즈니스 규칙을 논의하여 카디널리티와 모달리티 결정
## ERD 작성 팁
•	DFD의 데이터 저장소는 일반적으로 ERD의 엔터티와 대응됨
•	두 개 이상의 인스턴스를 가지는 엔터티만 포함
•	시스템 구현과 관련된 엔터티는 포함하지 말 것 (예: 오래된 데이터의 보관 파일) → 나중에 추가 가능
## 고급 문법 - 교차 엔터티 (Intersection Entities)
•	두 개의 엔터티가 M:N 관계를 가질 때, 이 정보를 저장하기 위해 새로운 엔터티 생성
	•	두 엔터티 사이의 M:N 관계를 제거하고, 중간에 새로운 엔터티 삽입
	•	두 개의 1:N 관계 생성: 기존 엔터티들이 부모가 되고 새로운 교차 엔터티가 자식이 됨
	•	교차 엔터티에 이름 부여
	•	부모 엔터티의 기본 키를 새로운 엔터티에 외래 키로 이식 (또는 연결 기본 키로 사용 가능)
### M:N Intersection Entity 풀기
![[Pasted image 20250521184929.png|300]]
![[Pasted image 20250521184939.png|300]]
## ERD 검증
품질 있는 데이터 모델을 위한 점검
### 설계 지침
•	엄격한 규칙보다는 모범 사례
•	엔터티는 다수의 인스턴스를 가져야 함
•	불필요한 속성은 피함
•	모든 구성 요소는 명확하게 라벨링
•	정확한 카디널리티와 모달리티 적용
•	속성은 필요한 최소 단위로 분해
•	라벨은 일반적인 비즈니스 용어를 반영
•	가정은 명확하게 명시
### ERD와 DFD의 균형
•	모든 분석 활동은 서로 연관됨
•	프로세스 모델에는 데이터 흐름과 데이터 저장소라는 두 가지 데이터 구성 요소가 있음
	•	DFD의 데이터 저장소와 흐름은 ERD의 엔터티 및 속성과 균형을 이루어야 함
•	많은 CASE 도구들이 불균형을 점검하는 기능 제공
•	모든 데이터 저장소 및 속성이 두 모델 간에 일치하는지 확인
•	사용되지 않는 데이터는 불필요
	•	누락된 데이터는 시스템을 불완전하게 만듦
	•	무비판적으로 따르지 말고 모델이 실제로 타당한지 확인
![[Pasted image 20250521205637.png|300]]
## 정규화
•	데이터 모델을 검증하기 위한 기법
•	논리 데이터 모델의 구성을 개선하기 위해 적용되는 일련의 규칙들
•	일반적으로 세 가지 정규화 규칙 사용
### 예제 1: 정규화되지 않은 엔터티
•	논리 데이터 모델에서 엔터티를 선택
![[Pasted image 20250521205843.png|400]]
하나의 엔터티 발생에 대해 어떤 속성(또는 속성들의 그룹)이 두 번 이상 나타나는가?
### 1차 정규형(1NF)
•	하나의 엔터티 인스턴스에서 속성(또는 속성 그룹)이 두 번 이상 발생하는가?
•	그렇다면 해당 속성(또는 그룹)을 별도의 엔터티로 분리
![[Pasted image 20250521205928.png|300]]
### 2차 정규형(2NF)
•	연결 키를 가진 엔터티에서, 어떤 속성이 전체 키가 아닌 일부 키에만 의존하는가?
•	그렇다면 해당 속성들을 새로운 엔터티로 이동
![[Pasted image 20250521205939.png|300]]
### 3차 정규형(3NF)
•	어떤 속성 값이 엔터티의 키가 아닌 다른 속성에 의존하는가?
•	그렇다면 해당 속성들을 새로운 엔터티로 이동
![[Pasted image 20250521205949.png|400]]