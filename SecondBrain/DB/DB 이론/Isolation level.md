# SQL 표준의 Isolation level
![[Pasted image 20250207170018.png|400]]
## read committed
read하는 시간을 기준으로 그 전에 commit된 데이터를 읽는다.
## repeatable read
tx 시작 시간 기준으로 그 전에 commit된 데이터를 읽는다.
같은 데이터를 여러 번 조회하더라도 동일한 결과를 보장받기 때문에 repeatable read이다.
## seriazable
-  mysql
	- MVCC로 동작하기 보다는 lock으로 동작한다
- postgresql
	- SSI(Serializable Snapshot Isolation) 기법이 적용된 MVCC로 동작한다.
## SNAPSHOT Isolation
표준에서 정의한 Isolation level과는 약간 다르며, Transaction 당 스냅샷을 만들어서 사용하는 것
![[Pasted image 20250207173212.png|400]]
commit시 snapshot을 DB에 반영한다.
하지만 write write conflict가 발생하면, 먼저 commit된 트랜잭션만 인정해준다.
따라서 Transaction 1은 abort 처리가 된다.
- tx 시작 전에 commit된 데이터만 보인다
- First-commiter win

![[Isolation level 별 문제]]