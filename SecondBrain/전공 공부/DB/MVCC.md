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
#### Lost update가 일어나는 read committed 상황
![[Pasted image 20250212174915.png|400]]
그 전에 commit된 데이터를 읽고 업데이트 하므로 lost update가 발생할 수 있다.
### postgreSQL 해결방안
#### repeatable read일 때
![[Pasted image 20250212175224.png|400]]
=> 같은 데이터에 먼저 update한 tx가 commit되면 이후 tx는 rollback하는 방식으로 해결 (두 개의 tx가 같은 데이터를 수정했을 때만 롤백 되는 것)
### MySQL 해결방안
#### repeatable read일 때
![[Pasted image 20250214171230.png|400]]
postgreSQL과 다르게 Locking read를 이용해 repeatable read에서의 Lost Update를 해결한다. (개발자가 SELECT시 따로 작성해야 한다.)
Locking read -> SELECT 맨 뒤에 개발자가 FOR UPDATE(exclusive lock) FOR SHARE(share lock)를 붙이면 Lock이 걸리면서 Lost Update를 방지할 수 있다.
## Write skew 문제
![[Pasted image 20250214171125.png|400]]
### postgreSQL & MySQL 해결 방안
개발자가 Locking read를 설정하면 두 DB 모두 Write Skew를 해결할 수 있지만, 내부 동작 방식은 약간 다르다
#### postgreSQL
![[Pasted image 20250214172047.png|400]]
Locking read를 통해 이전 tx에서 x를 update했으므로 나중 tx가 rollback되어 Write Skew를 해결할 수 있게 된다.
#### MySQL
![[Pasted image 20250214172215.png|400]]
Locking read를 통해 Write Skew를 해결할 수 있다.
