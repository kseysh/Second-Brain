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