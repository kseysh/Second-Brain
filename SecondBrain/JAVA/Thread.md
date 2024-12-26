```java
class MyThread extends Thread {
	@Override
	public void run(){
		// 작업 내용
	}
}

class MyThread2 implements Runnable {
	public void run(){
		// 작업 내용
	}
}

class ThreadTest {
	public static void main(String args[]){
		Mythread t1 = new MyThread();
		t1.start(); // start로 쓰레드를 실행한다
	}
}
```
![[Pasted image 20241226210438.png|400]]
## 실행 제어
![[Pasted image 20241226214309.png]]
- resume, stop, suspend는 쓰레드를 교착상태로 만들기 쉽기 때문에 deprecated 되었다.
## 스레드의 상태
![[Pasted image 20241226214501.png|400]]
## 스레드의 실행제어 메서드
### `sleep`
현재 스레드를 지정된 시간동안 TIMED_WAITING 상태로 만든다.
InterruptedException이 발생하면 깨어나므로 예외처리를 해야 한다.
특정 스레드를 지정해서 멈추게 하는 것은 불가능하다.
### `interrupt`
WAINTING인 스레드를 RUNNABLE 상태로 만든다.
### `suspend, resume, stop`
스레드의 실행을 일시정지, 재개, 완전정지 시킨다.
교착상태에 빠지기 쉬워 Deprecated 되었다.
### `yield`
남은 시간을 다음 스레드에게 양보하고, 자신은 실행대기한다.
yield와 interrupt를 적절히 사용하면 응답성과 효율을 높일 수 있다.
### `join`
지정된 시간동안 특정 스레드가 작업하는 것을 기다린다
예외처리를 해야 한다.