## vertical partitioning
테이블의 각 필드를 나누어서 저장하는 것.
정규화가 vertical partitioning의 방법 중 일부이다.
select문을 통해 데이터를 조회하게 되면 where문에서 데이터를 찾아서, 모든 데이터를 들고있다가 select문에서 필요하다고 한 데이터를 필터링해서 제공하는데, 이 때 필요없는 데이터도 들고 있게 된다.
이렇게 되면 풀 스캔시에 부하가 많이 발생할 수 있다.

#### vertical 
이를 해결하기 위해 vertical partitioning을 사용할 수 있는데, 예를 들어 게시판을 만들게 되면 게시판 목록에는 content가 필요없지만, content 특성상 데이터가 크다. 
이 때 content를 게시판 테이블에서 분리한다면 게시판 조회시에 content를 사용하지 않고 조회가 가능하여 조회 속도가 빨라질 수 있다.
![[Pasted image 20250219180750.png|400]]

## horizontal partitioning
