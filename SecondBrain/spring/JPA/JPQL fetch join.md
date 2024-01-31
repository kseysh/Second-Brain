# fetch join이란?
- SQL 조인의 종류가 아니다.
- JPQL에서 성능 최적화를 위해 제공하는 기능으로 연관된 엔티티나 컬렉션을 SQL 한 번에 함께 조회하는 기능이다.
- join fetch 명령어로 사용할 수 있다.
	  ex) `[left [outer] | inner] join fetch 조인경로`

## 엔티티 페치 조인
회원을 조회하면서 SQL 한 번에 연관된 팀도 함께 조회하고 싶을 때 사용한다.
SQL을 보면 회원 뿐만 아니라 팀도 함께 SELECT한다.
ex) `select m from Member m join fetch m.team`
=> `select M.*, T.* from MEMBER M inner join TEAM T on M.TEAM_ID=T.ID`

## 컬렉션 페치 조인
일대다 관계, 컬렉션 페치 조인

ex) `select t from Team t join fetch t.members where t.name = '팀A'`
=> `select T.*, M.* from TEAM T inner join MEMBER M on T.ID=M.TEAM_ID where T.NAME = '팀A'`

### 컬렉션 페치 조인에서 주의해야할 점
위 예시 코드에서 Team을 조회할 때,

![[Pasted image 20240131212534.png]]
위 사진처럼 한 팀에 여러 멤버가 있을 때 JOIN을 하게 되면

![[Pasted image 20240131212640.png]]
위 사진과 같이 조회되게 된다.

![[Pasted image 20240131212703.png]]
따라서 JOIN을 통해 TEAM을 조회하면 같은 team이 두 번 조회되게 된다.

![[Pasted image 20240131212811.png]]
위 사진을 보면 같은 팀 A가 두 번 조회되는 모습을 확인할 수 있다.

> 이 일