# 추상클래스 (abstract class)
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

# 추상메서드 (abstract method)

- 선언부만 있고 구현부 (몸통, body)가 없는 메서드
- 꼭 필요하지만 자손마다 다르게 구현될 것으로 예상되는 경우에 사용
- 추상클래스를 상속받는 자손클래스에서 추상메서드의 구현부를 완성해야한다.

## 추상클래스의 작성
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
	void move(int x, int y) { 이동하는 코드 } // 추상메서드가 호출되는 것이 아니라 각 자손들에 실제로 구현된 move 함수가 호출된다.
	void stimPack() { 스팀팩 사용 }
}

class Tank extends Unit {
	int x, y;
	void move(int x, int y) { 이동하는 코드 }
	void changeMode() { 공격모드 변환 }
}

```