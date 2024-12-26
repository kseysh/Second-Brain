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
