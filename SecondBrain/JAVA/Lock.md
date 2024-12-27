synchronized 블럭이라는 하나의 영역에서 한 스레드가 작업을 하려고 하면 다른 스레드는 접근할 수 없도록 누군가가 lock을 컨트롤해주어야 한다.
이를 자바에서는 lock(`intrinsic lock - 고유 락`), `monitor lock`, `monitor`라고 부른다.
이 락은 모든 객체에 하나씩 존재한다.

## 락의 단위
Object 레벨의 락 이외에도 다른 클래스 레벨의 락이 존재한다.
### Object level lock
```java
private void syncBlockMethod1(String msg) { 
	synchronized (this) { 
		//this level 
	} 
}
```
### Class level lock
```java
private void syncBlockMethod1(String msg) { 
	synchronized (Block1.class) { 
		//Class level 
	} 
}
```

