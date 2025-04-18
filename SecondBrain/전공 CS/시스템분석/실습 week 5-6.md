[주소](https://stupendous-butterkase-226.notion.site/CSE3308-System-Analysis-Hands-on-class-week-5-6-1c734e4a084080b9ad61f5856a523d31)
![[Pasted image 20250402181353.png|300]]

## Goals
(1) 요구사항을 토대로 ShoppingWebsite 모델의 **유즈케이스 시나리오**를 작성합니다.
(2) 유즈케이스 시나리오를 기반으로 **TDD**를 적용 해 봅니다.
(3) 1과 2를 참고하여 **Modified Sequence Diagram**을 작성합니다.
## Base Knowledge
- **Sequence Diagram (시퀀스 다이어그램)** 문제 해결을 위한 객체를 정의하고 객체간의 상호작용 메시지 시퀀스를 시간의 흐름에 따라 나타내는 다이어그램

| **객체와 생명선(Lifeline)**         | **활성 박스(Activation Box)** |
| ----------------------------- | ------------------------- |
| - 객체(활동 주체)는 직사각형으로 표현        | - 활성(Activation)          |
| - 라이프라인은 객체에서 이어지는 점선으로 표현    | - 생명선상에서 길다란 직사각형으로 표현    |
| - 점선은 위에서 아래로 갈 수록 시간의 경과를 의미 | - 현재 객체가 어떤 활동을 하고 있음을 의미 |

![[Pasted image 20250402181426.png|200]]![[Pasted image 20250402181437.png|200]]![[Pasted image 20250418164909.png|200]]
![[Pasted image 20250418164938.png|200]] 


| **유형**                       | **의미**                                                                              |
| ---------------------------- | ----------------------------------------------------------------------------------- |
| 동기 메시지(Synchronous message)  | 메시지 전송 객체가 계속하기 전까지 동기 메시지에 대한 응답을 기다림. 프로그램 내 일반적인 함수 호출과 동일한 동작 방식의 메시지를 표현       |
| 비동기 메시지(Async message)       | 메시지 전송 객체가 계속하기 전까지 응답을 요구하기 않는 메시지. 전송 객체의 호출만을 표시.보통 개별 쓰레드 간의 통신 및 새 쓰레드의 생성에 사용 |
| 자체 메시지(Self message)         | 자신에게 보낸 메시지입니다. 결과로 생성된 실행 발생이 전송 실행 위에 나타남.                                        |
| 반환 메시지(Reply/Return message) | 이전 호출의 반환을 기다리는 객체에게 다시 반환되는 메시지.                                                   |

> [시퀀스 다이어그램 - IT위키 (itwiki.kr)](https://itwiki.kr/w/%EC%8B%9C%ED%80%80%EC%8A%A4_%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8)

### Discussion

- **Test Driven Development (테스트 주도 개발)** 테스트 케이스를 먼저 작성하고, 이를 통과하는 코드를 작성하는 개발 방법론. (Agile apporach)

## Requirements of model design

- 온라인 쇼핑몰에서 고객이 상품을 주문하는 시스템을 설계하고자 한다. 해당 시스템의 구성요소는 다음과 같다:
    1. 소비자는 쇼핑몰 시스템에게 상품 정보를 요청할 수 있어야한다. 요청에 대한 반환 결과는 상품(items)의 목록이다.
    2. 소비자는 쇼핑몰 시스템에게 특정 상품을 장바구니에 추가하도록 요청할 수 있어야한다. 쇼핑몰 시스템은 장바구니에 유저정보 및 아이템 정보를 전달한다.
    3. 소비자는 장바구니에 담긴 물품을 결제할 수 있어야 하며, 결제 전에 장바구니에 넣은 물건들과 가격 정보를 알 수 있어야한다.

### Exercise 1. 핵심 컴포넌트(액터) 분리 및 유즈케이스 시나리오 작성
- Summary, Relationship, Flow
### Apply Test Driven Development
- 요구사항 검증을 위한 테스트 케이스의 작성
- TC 통과를 위한 최소한의 코드 작성
- 코드를 표준에 맞게 리팩토링
### Exercise 2. 각 Actor를 Object로 하는 테스트 케이스 작성
- Condition(Trigger), Action, Expected Results(Postcondition)

**[Sample Sequence Diagram]**
![[Pasted image 20250402181613.png|400]]

### Model State

- 예시로 든 시퀀스 다이어그램에는 하나의 객체가 다른 객체에게 메시지를 보내고 응답을 돌려받기 위해 기다리는 Activity가 존재합니다. 예를들어, User가 Shopping mall로부터 상품 목록을 반환 받는 동안에는 어떠한 행동도 수행 할 수 없습니다.
- 이 경우 User request → System response 프로세스 동안 User의 State는 (대기) 라고 생각 해 볼 수 있습니다. 초기 상태 또한 존재합니다. 초기 상태는 (쇼핑몰 접속 상태) 라고 볼 수 있습니다.

---

## Exercise 3. Draw Modified Sequence Diagram(Sequence + State Transition)

- 이제 각 Object의 Lifeline은 Model의 State를 함께 표현합니다.
- 각 Object는 상태가 바뀌는 순간 Message를 보냅니다.
- 각 Object는 Message를 받거나, 내부 조건을 만족하면 상태가 바뀝니다.

**[Sample : IR Door]**
![[Pasted image 20250402181633.png|400]]
## HW 3

- 작성하였던 시나리오를 기반으로 { User/ShoppingMall/ItemManagement/Cart/Payment }객체들에 대한 Modifed Sequence Diagram을 완성하여 제출합니다.