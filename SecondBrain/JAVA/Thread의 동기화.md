한 번에 하나의 스레드만 객체에 접근할 수 있도록 객체에 락을 걸어서 데이터의 일관성을 유지하는 것

### 특정한 객체에 lock을 걸고자 할 때
```java
synchronized(객체의 참조변수){
	// ...
}
```
### 메서드에 lock을 걸고자 할 때
```java
public synchronized void calcSum(){
	//...
}
```

## `wait`, `notify`, `notifyAll`을 이용한 동기화
Object 클래스에 정의되어 있으며, 동기화 블록 내에서만 사용할 수 있다.
- `wait` - 객체의 lock을 풀고 스레드를 해당 객체의 waiting pool에 넣는다.
- `notify` - waiting pool에서 대기중인 스레드 중의 하나를 깨운다.
- `notifyAll` - waiting pool에서 대기중인 모든 스레드를 깨운다.