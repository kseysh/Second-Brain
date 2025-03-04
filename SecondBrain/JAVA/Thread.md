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
		// t1.run()을 사용하면 main에서 실행되므로 주의!
	}
}
```
생성한 스레드를 실행하려면 start를 이용해야 한다.

![[Pasted image 20241226210438.png|400]]
## 실행 제어
![[Pasted image 20241226214309.png]]
- resume, stop, suspend는 쓰레드를 교착상태로 만들기 쉽기 때문에 deprecated 되었다.
## 스레드의 상태
![[Pasted image 20241227202126.png|400]]

| **상태**   | **열거 상수**     | **설명**                         |
| -------- | ------------- | ------------------------------ |
| 객체 생성    | NEW           | 스레드 객체가 생성, 아직 start() 메소드 미호출 |
| 실행 대기    | RUNNABLE      | 실행 중인 상태 (다른 리소스를 기다리는 상태도 포함) |
| 일시 정지    | WAITING       | 다른 스레드가 알려줄 때까지 대기             |
| 시간 제한 대기 | TIMED_WAITING | 주어진 시간동안만 대기                   |
| 차단된 상태   | BLOCKED       | 사용하고자 하는 객체의 Lock이 걸려 대기하는 상태  |
| 종료       | TERMINATED    | 실행이 끝남                         |
## 데드락
![[Pasted image 20241227202300.png|400]]
서로 자원을 점유하고 서로 기다리고 있어 아무것도 하지 못하는 상태

데드락이 발생하는 조건은 아래와 같다.

| 조건    | 설명                                                             |
| ----- | -------------------------------------------------------------- |
| 상호 배제 | 자원은 한번에 한 프로세스만 사용할 수 있어야 함                                    |
| 점유 대기 | 최소한 하나의 자원을 점유하고 있으면서 다른 프로세스에 할당된 자원을 추가 점유 위해 대기             |
| 비선점   | 다른 프로세스에 할당된 자원은 뺏을 수 없음                                       |
| 순환 대기 | 프로세스의 집합에서 (P0~ PN) 이전 프로세스가 다음 프로세스가 점유한 자원을 대기하는 상태를 연달아서 가짐 |
