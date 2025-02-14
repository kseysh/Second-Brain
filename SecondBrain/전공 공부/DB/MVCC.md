MVCC = multiversion concurrency control
동시성으로 인해 데이터가 이상한 행동을 하지 않도록 control하는 것. Isolation level별로 특정 문제들을 제어할 수 있다.
## MVCC 특징
- MVCC는 commit된 데이터만 읽는다 
	- 데이터를 읽을 때 특정 시점 기준으로 가장 최근에 commit된 데이터를 읽는다.
- recovablity를 위해 commit 후에 write lock을 unlock한다.
- read와 write가 서로를 block하지 않는다.
- 데이터 변화 이력을 관리한다.

## Isolation level별 특징
### read committed
read하는 시간을 기준으로 그 전에 commit된 데이터를 읽는다.
### repeatable read
tx 시작 시간 기준으로 그 전에 commit된 데이터를 읽는다.
같은 데이터를 여러 번 조회하더라도 동일한 결과를 보장받기 때문에 repeatable read이다.
### seriazable
-  mysql
	- MVCC로 동작하기 보다는 lock으로 동작한다
- postgresql
	- SSI(Serializable Snapshot Isolation) 기법이 적용된 MVCC로 동작한다.

## lost update 문제
### postgreSQL
=> 같은 데이터에 먼저 update한 tx가 commit되면 이후 tx는 rollback된다.
#### read committed일 때
![[Pasted image 20250212174915.png|400]]
그 전에 commit된 데이터를 읽고 업데이트 하므로 lost update가 발생할 수 있다.
#### repeatable read일 때
![[Pasted image 20250212175224.png|400]]
### MySQL
