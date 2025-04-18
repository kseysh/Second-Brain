## Target

- **식당 전자결제(키오스크) 시스템에 대한 시스템 분석 및 컴포넌트 다이어그램 만들기**

1. 키오스크는 매장 내 주문 알림 시스템과 연동되어, 주문이 완료되면 주문 상품과 수량, 대기 번호를 알려줄 수 있어야 한다.
2. 키오스크는 카드 결제 시스템을 갖추고 있으며, 카드 결제 시스템은 외부 시스템을 사용하고 있으므로 결제가 성공했는지 아닌지만 판단하면 된다.
3. 키오스크는 영수증 출력 시스템을 갖추고 있다.

![[Pasted image 20250402180842.png|300]]

### Sub Tasks (15 ~ 20 min)

- 키오스크 시스템 분해
- 각 컴포넌트에 대한 유즈케이스 대략적으로 정의하기

## SRP (Single Responsibility)

- **단일 책임 원칙**
    - 하나의 컴포넌트(혹은 Class)가 하나의 **책임**만을 가져야 함
        - **왜** : (쉽게 말하면) 코드 길어지고 꼬이니까 → 시스템 복잡해져서

## **Component Diagram**
- 모델의 컴포넌트와 각 컴포넌트간의 인터페이스 및 포트를 모델링
- UML 규격을 준수하는 경우 (Provide/Required) Interface, Dependency 등에 대한 섬세한 표현이 필요하지만 본 강의에서는 해당 부분까지 다루지 않습니다.
![[Pasted image 20250402181121.png|400]]

## Role of Component Diagram

- 시스템의 물리적 / 논리적구조를 표현
- 시스템을 구성하는 각 요소들의 역할과 (필요한 경우) 의존성을 표현
- 시스템 인터페이스 설계에 활용
- 시스템 아키텍처의 구조를 분석
![[Pasted image 20250402181056.png|300]]

- 기본적으로는 전부 똑같은 컴포넌트임 (설명하는 수준만 다를 뿐)

## 예시 : 도서관 출입 시스템

- 도서관 출입 관리 시스템
    
    - 카드 리더기 시스템
        - 카드 읽기
        - 사용자 정보 조회 요청
    - 이용자 정보 조회 시스템
        - 카드 정보 검증
        - 유효 사용자 확인
    - DB
- 대략적인 컴포넌트 구성 표현법에서


그냥 네모 박스랑 저 사탕, 요청만 잘 그리면 됨
![[Pasted image 20250402181041.png|300]]

- 본격적인 컴포넌트 구성 표현법에서
![[Pasted image 20250402180911.png|300]]
동그라미는 provide, 감싸는 것은 Required를 뜻함


- UML 계층적 표현 단위에서
![[Pasted image 20250402180947.png|400]]
> 그러나 이렇게까지 복잡한 컴포넌트 다이어그램을 작성하지는 않습니다. 시스템이 너무나도 복잡한게 아니면 불필요하니까. → 좀 더 고전적으로 가면 추상메서드(**인터페이스**)와 **구현체**를 분리

![[Pasted image 20250402181002.png|400]]

- 또 다른 예시
![[Pasted image 20250402181023.png|400]]