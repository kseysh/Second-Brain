# fork & join framework
- 작업을 여러 스레드가 나눠서 처리하는 것을 쉽게 해준다
- `RecursiveAction` 또는 `RecursiveTask`를 상속받아서 구현한다
	- `RecursiveAction` - 반환 값이 없는 작업을 구현할 때 사용
	- `RecursiveTask` - 반환 값이 있는 작업을 구현할 때 사용
### `compute()`의 구현
상속을 통해 compute 메서드를 구현해야 한다.
- 수행할 작업과 작업을 어떻게 나눌 것인지를 정해줘야 한다
- `fork()`로 나눈 작업을 큐에 넣고, compute()를 재귀호출한다.
![[Pasted image 20241227215130.png|400]]
- `compute()`는 작업을 나눈다
- `fork()`는 작업을 큐에 넣는다 
	- 해당 작업을 스레드 풀의 작업 큐에 넣는다(비동기 메서드)
- `join()`으로 작업의 결과를 합친다. 
	- 해당 작업의 수행이 끝날 때까지 기다렸다가, 수행이 끝나면 그 결과를 반환한다 (동기 메서드)
### 작업 훔치기
- 작업을 나눠서 다른 스레드의 작업 큐에 넣는 것
![[Pasted image 20241227215143.png|200]]
## ForkJoinPool
### `invoke`
순차적인 코드에서 병렬 작업을 시작할 때 사용된다. 따라서 현재 스레드가 바로 작업을 실행하고 결과를 반환할 때까지 기다린다. 따라서 invoke를 사용하면 해당 작업은 비동기적으로 실행되지 않고 호출한 스레드에서 직접 실행되어 병렬 처리의 이점을 활용하지 못한다.

따라서 RecursiveTask (반환 값이 있는 작업을 구현할 때 사용)를 사용할 때는 ForkJoinPool의 invoke 메서드를 사용하지 말아야 한다. 대신 compute나 fork 메서드를 직접 호출할 수 있다. 순차 코드에서 병렬 계산을 시작할 때만 invoke를 사용한다.

### invoke의 사용 예시
```java
import java.util.concurrent.RecursiveTask;
import java.util.concurrent.ForkJoinPool;

public class Fibonacci extends RecursiveTask<Integer> {
    private final int n;

    public Fibonacci(int n) {
        this.n = n;
    }

    @Override
    protected Integer compute() {
        if (n <= 1) {
            return n;
        }
        Fibonacci f1 = new Fibonacci(n - 1);
        f1.fork(); // 새로운 작업을 병렬로 실행
        Fibonacci f2 = new Fibonacci(n - 2);
        return f2.compute() + f1.join(); // 결과를 병합
    }

    public static void main(String[] args) {
        ForkJoinPool pool = new ForkJoinPool();
        Fibonacci task = new Fibonacci(10);
        int result = pool.invoke(task); // 작업을 실행하고 결과를 반환
        // 이렇게 순차 코드에서 병렬 작업을 시작할 때 invoke 메서드를 사용하는 것이 좋다.
        System.out.println(result);
    }
}
```