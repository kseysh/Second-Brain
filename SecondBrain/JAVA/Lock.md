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

private synchronized void syncMethod1(){
	// this level
} 
```
모든 인스턴스가 각자의 lock을 가진다.
### Class level lock
```java
private void syncBlockMethod1(String msg) { 
	synchronized (Block1.class) { 
		//Class level 
	} 
}

private static synchronized void syncMethod1(){
	// Class level
} 
```
클래스의 인스턴스가 여러개 있어도 하나의 lock을 가진다. 클래스 하나당 하나의 lock을 가질 수 있다.

