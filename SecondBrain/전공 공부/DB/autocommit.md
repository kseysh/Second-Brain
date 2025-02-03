- 각각의 SQL문을 자동으로 transaction 처리 해주는 개념
- SQL문이 성공적으로 실행하면 자동으로 commit 한다
- 실행 중에 문제가 있었다면, 알아서 rollback 한다
- MySQL에서는 default로 autocommit이 enabled 되어 있다
- 다른 DBMS에서도 대부분 같은 기능을 제공한다.
- START TRANSACTION 실행과 동시에 autocommit은 off 된다.
- COMMIT / ROLLBACK과 함께 transaction이 종료되면 원래 autocommit 상태로 돌아간다.


## @Transactional의 의미

![[Pasted image 20250203183210.png|400]]
이를 단순화 시킨 것이 @Transactional
