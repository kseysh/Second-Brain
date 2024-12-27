# fork & join framework
- 작업을 여러 스레드가 나눠서 처리하는 것을 쉽게 해준다
- `RecursiveAction` 또는 `RecursiveTask`를 상속받아서 구현한다
	- `RecursiveAction` - 반환 값이 없는 작업을 구현할 때 사용
	- `RecursiveTask` - 반환 값이 있는 작업을 구현할 때 사용
### `compute()`의 구현
상속을 통해 compute 메서드를 구현해야 한다.
- 수행할 작업과 작업을 어떻게 나눌 것인지를 정해줘야 한다
- `fork()`로 나눈 작업을 큐에 넣고, compute()를 재귀호출한다.