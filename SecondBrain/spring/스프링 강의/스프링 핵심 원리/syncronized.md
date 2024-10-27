# syncronized란?
자바에서는 멀티스레드를 활용하면 여러 작업을 동시에 처리할 수 있어 작업 효율이 증가한다.
하지만 하나의 공유 자원을 여러 스레드에서 동시에 접근하여 사용하면 안되므로 이 때 스레드 동기화를 사용해 여러 스레드가 하나의 공유 자원에 동시에 접근하지 못하도록 막는다.

공유데이터가 사용되어 동기화가 필요한 부분을 임계영역(critical section)이라고 부르며, 자바에서는 이 임계영역에 **synchronized 키워드**를 사용하여 여러 스레드가 동시에 접근하는 것을 금지함으로써 동기화를 할 수 있습니다. 

## 사용 방법
synchronized 키워드는 동기화가 필요한 메소드나 코드블럭앞에 사용하여 동기화 할 수 있습니다. synchronized로 지정된 임계영역은 한 스레드가 이 영역에 접근하여 사용할때 lock이 걸림으로써 다른 스레드가 접근할 수 없게 됩니다. 이후 해당 스레드가 이 임계영역의 코드를 다 실행 후 벗어나게되면 unlock 상태가 되어 그때서야 대기하고 있던 다른 스레드가 이 임계영역에 접근하여 다시 lock을 걸고 사용할 수 있게 됩니다. 

 lock은 해당 객체당 하나씩 존재하며, synchronized로 설정된 임계영역은 lock 권한을 얻은 하나의 객체만이 독점적으로 사용하게 된다. 

### 메소드에 synchronized 설정하기
메소드 이름 앞에 synchronized 키워드를 사용하면 해당 메소드 전체를 임계영역으로 설정할 있다. 
```java
synchronized void increase() {
	count++;
	System.out.println(count);
}
```

### 코드블럭에 synchronized 설정하기
동기화를 많이 사용하게 되면 효율이 떨어지게 되므로 꼭 필요한 부분에만 블럭을 지정하여 임계영역으로 설정할 수 있다. 
예제와 같이 synchronized(this)로 지정하게 되면 참조변수(this) 객체의 lock을 사용하게 된다. 
```java
void increase() {
	synchronized(this) {
		count++;
	}
	System.out.println(count);
}
```
