## Controller
### 컨트롤러 클래스 안에서 메서드 명을 작성 할 때는 아래와 같은 접미사를 붙인다.
- orderList() – 목록 조회 유형의 서비스
- orderDetails() – 단 건 상세 조회 유형의 controller 메서드
- orderSave() – 등록/수정/삭제 가 동시에 일어나는 유형의 controller 메서드
- orderAdd() – 등록만 하는 유형의 controller 메서드
- orderModify() – 수정만 하는 유형의 controller 메서드
- orderRemove() – 삭제만 하는 유형의 controller 메서드](<Assert.assertEquals("주문 취소시 상태는 CANCEL 이다.", OrderStatus.CANCEL, getOrder.getStatus());>)

## Service
### 서비스 클래스 안에서 메서드 명을 작성 할 때는 아래와 같은 접두사를 붙인다.

- findOrder() - 조회 유형의 service 메서드
- addOrder() - 등록 유형의 service 메서드
- modifyOrder() - 변경 유형의 service 메서드
- removeOrder() - 삭제 유형의 service 메서드
- saveOrder() – 등록/수정/삭제 가 동시에 일어나는 유형의 service 메서드

## Mapper
### Mapper 클래스 안에서 메서드 명을 작성할 때는 아래와 같은 접두사를 붙인다.
- selectOrder() - 조회 유형의 mapper 메서드
- insertOrder() - 등록 유형의 mapper 메서드
- updateOrder() – 변경 유형의 mapper 메서드
- deleteOrder() - 삭제 유형의 mapper 메서드

## Structure

1. 패키지는 목적별로 묶어 생성한다.
    - common(공통기능 관련), user(유저 관련), Order(주문관련) ....

2. Controller에서는 Service 호출과 Exception 처리만을 담당한다.
    - Controller에서의 비즈니스 로직 구현은 최대한 피한다.

3. 메소드와 클래스는 하나의 목적만을 위해 생성한다.
    - 한개의 메소드는 한가지의 기능만을 가져야 한다.
    - 한개의 클래스 내부에는 같은 목적만을 가진 코드가 존재하여야한다.

4. 메소드와 클래스는 가능한 작게만든다.
    - 여러 기능이 모인 적은수의 큰 클래스보다는 목적이 뚜렷한 작은 클래스 여러개로 이루어진 시스템이 바람직하다.

5. 도메인명의 Service 생성은 피한다.
    - Order 라는 도메인이 있을 때 OrderService 로 만드는 것은 피한다.
    - OrderService로 만들 경우 그 안에 도메인과 관련된 여러 기능을 넣을 가능성이 높다.
    - 도메인 관련 기능을 세분화하여 Service를 만든다(ex. OrderRegisretService, OrderStatusService .....)

## 오라클에서의 자바 코드 컨벤션
https://www.oracle.com/java/technologies/javase/codeconventions-introduction.html
