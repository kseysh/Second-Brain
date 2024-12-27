자바 변수를 Main Memory에 저장하겠다라는 것을 명시한다.
매번 변수의 값을 Read/Write할 때마다 CPU cache에 저장된 값이 아닌 main memory에서 읽고 쓴다.

- 성능 향상을 위해 변수의 값을 core의 cache에 저장해 놓고 작업한다.
- 여러 스레드가 공유하는 변수에는 volatile을 붙여야 항상 메모리에서 읽어온다.

![[Pasted image 20241227212747.png|200]]
![[Pasted image 20241227212739.png|400]]
volatile 변수를 사용하고 있지 않은 MultiThread 환경에서는 Task를 수행하는 동안 성능 향상을 위해 main memory에서 읽은 변수를 CPU cache에 저장한다.
만약, Multi Thread 환경에서 Thread가 변수 값을 읽어올 때 각각의 CPU cache에 저장된 값이 다르기 때문에 변수 값 불일치 문제가 발생하게 된다.

## 사용하는 곳
Multi Thread 환경에서 하나의 Thread만 read & write하고 나머지 Thread가 read하는 상황에서 가장 최신의 값을 보장한다.

하지만, 하나의 Thread가 아닌 여러 Thread가 write하는 상황에서는 동시성 이슈가 발생할 수 있으므로 여러 Thread가 write하는 상황이라면, synchronized를 통해 변수 read & write의 원자성을 보장해야 한다.
