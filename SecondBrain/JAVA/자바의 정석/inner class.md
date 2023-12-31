# inner class란?
- 클래스 안에 선언된 클래스
- 특정 클래스 내에서만 주로 사용되는 클래스를 내부 클래스로 선언한다.
- GUI 어플리케이션(AWT, Swing)의 이벤트처리에 많이 사용된다.

# 내부 클래스의 장점
- 내부 클래스에서 외부 클래스의 멤버들을 쉽게 접근할 수 있다.
- 코드의 복잡성을 줄일 수 있다. (캡슐화)
## 내부 클래스가 캡슐화에 도움을 주는 이유
- 접근 제어
내부 클래스는 외부 클래스에 private 멤버에도 쉽게 접근함으로써 캡슐화를 강화할 수 있다.
- 코드 정리
내부 클래스를 이용해 관련된 코드를 그룹화하고 구조화 할 수 있으며 코드의 가독성과 유지 보수성을 향상시킬 수 있다.
- 정보 은닉
내부 클래스로 외부에서 내부 클래스의 세부구현을 숨길 수 있다.
- 콜백 구현
내부 클래스로 콜백 구현을 간단하게 할 수 있다. 콜백은 어떤 이벤트가 발생할 때 특정 동작을 수행하는 데 사용된다. 내부 클래스를 통해 이벤트 리스너나 콜백 인터페이스를 구현하면 외부 클래스와의 상호작용이 쉽고 간편해진다.

# 내부 클래스의 종류와 특징
- 내부 클래스의 종류는 변수의 선언위치에 따른 종류와 동일하다.
- 유효범위와 성질도 변수와 유사하므로 비교해보면 이해하기 쉽다.         

| 내부 클래스     | 특징                                                                                                                                                                   |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 인스턴스 클래스 | 외부 클래스의 멤버변수 선언위치에 선언하며, 외부 클래스의 인스턴스멤버처럼 다루어진다. 주로 외부 클래스의 인스턴스 멤버들과 관련된 작업에 사용될 목적으로 선언된다.    |
| 스태틱 클래스   | 외부 클래스의 멤버변수 선언위치에 선언하며, 외부 클래스의 static 멤버처럼 다루어진다. 주로 외부 클래스의 static 멤버, 특히 static 메서드에서 사용될 목적으로 선언된다. |
| 지역 클래스     | 외부 클래스의 메서드나 초기화 블럭 안에 선언하며, 선언된 영역 내부에서만 사용될 수 있다.                                                                               |
| 익명 클래스     | 클래스의 선언과 객체의 생성을 동시에 하는 이름없는 클래스                                                                                                              |

## 내부 클래스의 제어자와 접근성
- 내부 클래스의 접근제어자는 내부 클래스의 변수에 사용할 수 있는 접근제어자와 동일하다.
- static 클래스만 static 멤버를 정의할 수 있다.
- 내부 클래스도 외부 클래스의 멤버로 간주되며, 동일한 접근성을 갖는다.




