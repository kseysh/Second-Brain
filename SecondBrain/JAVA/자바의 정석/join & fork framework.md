작업을 여러 스레드가 나눠서 처리하는 것을 쉽게 해준다.
RecursiveAction 또는 RecursiveTask를 상속받아서 구현
![[Pasted image 20240429105044.png]]
## compute()의 구현
수행할 작업과 작업을 어떻게 나눌 것인지를 정해줘야 한다.
fork()로 나눈 작업을 큐에 넣고, compute()를 재귀호출한다.
![[Pasted image 20240429105132.png]]
## work stealing
작업을 나눠서 다른 쓰레드의 작업 큐에 넣는 것
![[Pasted image 20240429105204.png]]
## fork()와 join()
compute()는 작업을 나누고, fork()는 작업을 큐에 넣는다.
join()으로 작업의 결과를 합친다.
![[Pasted image 20240429110017.png]]

