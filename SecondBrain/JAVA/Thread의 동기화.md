한 번에 하나의 스레드만 객체에 접근할 수 있도록 **객체에 락을 걸어**서 데이터의 일관성을 유지하는 것
### 특정한 객체에 [[Lock]]을 걸고자 할 때
*블럭의 영역 안으로 들어가는 시점부터 객체의 Lock을 얻어* 작업을 수행한다.
```java
synchronized(객체의 참조변수){
	// ...
}
```
#### synchronized(this)
this는 클래스의 인스턴스 메모리 주소를 가리킨다.
synchronized 블록에 this를 사용하면 모든 synchronized 블록에 하나의 객체에 같은 락을 사용하므로 한 synchroinzed 블럭이 끝나고 락이 반환될 때까지 다른 모든 synchronized 블록은 사용할 수 없다.
#### synchronized(Object)
this가 아닌 객체를 따로 만들어 블럭이 갖는 객체의 락을 다르게 해주면 synchronized 블록은 서로 동시에 실행할 수 있다. 그리고 이 때 만드는 o1, o2는 락을 위해서만 사용하는 임시 객체이므로 어느것으로 만들어도 상관없다.
### 메서드에 lock을 걸고자 할 때
synchronized *메서드가 호출된 시점부터 객체의 lock을 얻어* 작업을 수행한다
```java
public synchronized void calcSum(){
	//...
}
```
=> 어떤 방식을 사용하던 lock을 거는 시점이 달라지는 것이다. 두 방식 모두 *공유 객체에 lock을 건다*.
한 스레드가 특정 공유 객체에 대해 lock을 걸면, 그 객체에 대해 lock을 요청하는 다른 스레드는 BLOCKED 상태가 된다.
## `wait`, `notify`, `notifyAll`을 이용한 동기화
Object 클래스에 정의되어 있으며, 동기화 블록 내에서만 사용할 수 있다.
- `wait` - *객체의 lock을 풀고* *스레드를 해당 객체의 waiting pool에 넣는다*. (waiting = waiting pool에 들어감으로 생각)
- `notify` - waiting pool에서 대기중인 스레드 중의 하나를 깨운다.
- `notifyAll` - waiting pool에서 대기중인 모든 스레드를 깨운다.

### 동기화 대표 예제
[[생산자와 소비자 문제]]