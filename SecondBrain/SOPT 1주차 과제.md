# 자바 제어자와 접근 제어자

# 제어자
## final

> final이 사용될 수 있는 곳 - 클래스, 메서드, 멤버변수, 지역변수

| 대상     | 의미                                                                                |
| -------- | ----------------------------------------------------------------------------------- |
| 클래스   | 변경될 수 없는 클래스, 확장될 수 없는 클래스가 된다.                                |
| 메서드   | 변경될 수 없는 메서드, final로 지정된 메서드는 오버라이딩을 통해 재정의 될 수 없다. |
| 멤버변수 & 지역변수 | 변수 앞에 final이 붙으면, 값을 변경할 수 없는 상수가 된다.                          |

> final이 붙은 변수는 상수이므로 보통은 선언과 초기화를 동시에 하지만, 인스턴스마다 고정값을 갖는 인스턴스 변수의 경우 생성자에서 초기화한다.

ex)
```
class Card{
	final int NUMBER;
	final String KIND;
	Card(String kind, int num){
		KIND = kind;
		NUMBER = num;
	}
}


--------------------

public static void main(String args[]){
	Card c = new Card("HEART", 10);
	// c.NUMBER = 5 => 에러 발생
}
```


## abstract

> abstract가 사용될 수 있는 곳 - 클래스, 메서드

### 추상클래스 (abstract class)
- 클래스가 설계도라면 추상클래스는 '미완성 설계도'
- 추상메서드(미완성 메서드)를 포함하고 있는 클래스

ex)
```
abstract class Player{
	int currentPos;

	Player() {                   // 추상클래스도 생성자가 있어야 한다.
		currentPos = 0;
	}

	abstract void play(int pos); // 추상메서드
	abstract void stop();        // 추상메서드

	void play(){
		play(currentPos);        // 추상메서드를 사용할 수 있다.
	}
}
```
- 일반메서드가 추상메서드를 호출할 수 있다. (호출할 때 필요한건 선언부)
- 완성된 설계도가 아니므로 인스턴스를 생성할 수 없다.
- 다른 클래스를 작성하는 데 도움을 줄 목적으로 작성된다.

### 추상메서드 (abstract method)

- 선언부만 있고 구현부 (몸통, body)가 없는 메서드
- 꼭 필요하지만 자손마다 다르게 구현될 것으로 예상되는 경우에 사용
- 추상클래스를 상속받는 자손클래스에서 추상메서드의 구현부를 완성해야한다.

### 추상클래스의 작성
- 여러 클래스에 공통적으로 사용될 수 있는 추상클래스를 바로 작성하거나 기존클래스의 공통 부분을 뽑아서 추상클래스를 만든다.

ex) 
```
class Marine {
	int x, y;
	void move(int x, int y) { 이동하는 코드 }
	void stop() { 정지하는 코드 }
	void stimPack() { 스팀팩 사용 }
}

class Tank {
	int x, y;
	void move(int x, int y) { 이동하는 코드 }
	void stop() { 정지하는 코드 }
	void changeMode() { 공격모드 변환 }
}

------------ ↓ -------------

abstract class Unit{
	int x, y;
	abstract void move(int x, int y);
	void stop();
}

class Marine extends Unit {
	int x, y;
	void move(int x, int y) { 이동하는 코드 } // 추상메서드가 호출되는 것이 아니라 각 자손들에 실제로 구현된 move 함가 호출된다.
	void stimPack() { 스팀팩 사용 }
}

class Tank extends Unit {
	int x, y;
	void move(int x, int y) { 이동하는 코드 }
	void changeMode() { 공격모드 변환 }
}

```


| 대상   | 의미                                                            |
| ------ | --------------------------------------------------------------- |
| 클래스 | 클래스 내에 추상 메서드가 선언되어 있음을 의미한다              |
| 메서드 | 선언부만 작성하고 구현부는 작성하지 않은 추상메서드임을 알린다. |

ex)
```
abstract class AbstractTest{
	abstract void move();
}
```


# 접근 제어자

> 접근 제어자가 사용될 수 있는 곳 - 클래스, 멤버변수, 메서드, 생성자

- private - 같은 클래스 내에서만 접근이 가능하다.
- default - 같은 패키지 내에서만 접근이 가능하다.
- protected - 같은 패키지 내에서, 그리고 다른 패키지의 자손 클래스에서 접근이 가능하다.
- public - 접근 제한이 전혀 없다.

### 접근제어자를 사용하는 이유
- 외부로부터 데이터를 보호하기 위해서
- 외부에는 불필요한, 내부적으로만 사용되는 부분을 감추기 위해서

# 불가능한 제어자 조합

## 메서드에서의 static + abstract
static 메서드는 몸통(구현부)이 있는 메서드에만 사용할 수 있다.
## 클래스에서 abstract+ final
- 클래스에 사용되는 final은 클래스를 확장할 수 없다는 의미이고, abstract는 상속을 통해서 완성되어야 한다는 의미이므로 서로 모순되기 때문이다.
## 메서드에서의 abstract +private
- abstract메서드는 자손클래스에서 구현해주어야 하는데 접근 제어자가 private이면, 자손클래스에서 접근할 수 없기 때문이다.
## 메서드에서의 private + final (불가능하지는 않지만 권장 X)
- 접근 제어자가 private인 메서드는 오버라이딩될 수 없기 때문이다. 이 둘 중 하나만 사용해도 의미가 충분하다.
