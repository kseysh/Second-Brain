1. 클래스 다중 상속 금지
	1. 다이아몬드 상속 문제가 발생한 메서드 상속의 모호성을 피한다.
2. interface를 활용한 다중 구현 허용
	1. interface는 구현이 없으므로 다중 상속시 메서드 충돌이 발생하지 않음
3. default 메서드가 충돌한다면 상속 메서드에서 반드시 재정의가 필요하다
4. is-a 상속 관계 대신 has-a 포함관계를 주로 사용한다.
```java
class Engine { void start() { System.out.println("Engine on"); } }
class Car { 
    private Engine engine = new Engine(); 
    void drive() { engine.start(); }
}
```