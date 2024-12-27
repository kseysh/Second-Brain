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
