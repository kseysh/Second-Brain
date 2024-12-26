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
