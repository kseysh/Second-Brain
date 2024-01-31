# fetch join이란?
- SQL 조인의 종류가 아니다.
- JPQL에서 성능 최적화를 위해 제공하는 기능으로 연관된 엔티티나 컬렉션을 SQL 한 번에 함께 조회하는 기능이다.
- join fetch 명령어로 사용할 수 있다.
	  ex) `[left [outer] | inner] join fetch 조인경로`

## 엔티티 페치 조인
회원을 조회하면서 SQL 한 번에 연관된 팀도 함께 조회하고 싶을 때 사용한다.
SQL을 보면 회원 뿐만 아니라 팀도 함께 SELECT한다.