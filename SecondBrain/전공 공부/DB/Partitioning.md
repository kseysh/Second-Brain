## vertical partitioning
테이블의Column을 기준으로 나누어 저장하는 것.
정규화가 vertical partitioning의 방법 중 일부이다.
자주 사용하는 데이터와 사용하지 않는 데이터를 나눠 파티션을 구성하면 성능을 향상시킬 수 있다.
#### vertical partitioning 예제
데이터 조회를 하면 where문에서 row의 모든 데이터를 찾고, 
select문에서 필요하다고 한 데이터를 필터링해서 제공한다.
이 때 필요 없는 데이터도 들고 있게 된다.
이렇게 되면 풀 스캔시에 부하가 많이 발생할 수 있다.

이를 해결하기 위해 vertical partitioning을 사용할 수 있다. 
예를 들어 게시판을 만들 때 게시판 목록에는 content가 필요없고, content 특성상 데이터가 크다.
이 때 content를 게시판 테이블에서 분리한다면 게시판 조회시에 content를 사용하지 않고 조회가 가능하여 조회 성능을 향상시킬 수 있다.
![[Pasted image 20250219180750.png|400]]
## horizontal partitioning
테이블의 크기가 커질 수록 데이터의 크기도 늘어난다. 그렇게 되면 아무리 인덱스를 사용하더라도 인덱스의 성능이 점점 나오지 않으며, 삽입 삭제 시에도 점차 시간이 오래 걸리게 된다. 이와 같은 상황에 horizontal partitioning을 진행한다.

특정 partitioning key를 설정해 hash function을 이용해 partitioning을 진행한다. 하지만 horizontal partitioning을 partition key가 아닌 key를 이용해 조회를 하려하면 full scan을 해버려야 하는 상황이 발생한다.
이 때