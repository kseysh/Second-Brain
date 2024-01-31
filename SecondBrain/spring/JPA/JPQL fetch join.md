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
**일대다 관계** 

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

> 이 문제를 해결하기 위해서는?

### DISTINCT
SQL의 DISTINCT는 중복된 결과를 제거하는 명령으로 JPQL의 DISTINCT는 2가지 기능을 제공한다.
- SQL에 DISTINCT를 추가
- 애플리케이션에서 엔티티 중복 제거

ex) `select distinct t from Team t join fetch t.members where t.name = '팀A'`
*but* SQL에 DISTINCT를 추가하지만 데이터가 다르므로 SQL 결과에서 중복제거를 실패한다.
![[Pasted image 20240131213257.png]]
DISTINCT로 제거되기 위해서는 완전히 같아야 하는데, ID값과 NAME값 달라 DISTINCT로 제거되지 않는다.

따라서 JPA가 추가로 애플리케이션에서 중복 제거를 시도하게 됩니다. 따라서 같은 식별자를 가진 Team 엔티티를 제거합니다. => 원했던대로 결과가 나오도록 도와줌
!! 하이버네이트6 부터는 DISTINCT 명령어를 사용하지 않아도 애플리케이션에서 중복 제거가 자동으로 적용됩니다 !!
## 페치 조인과 일반 조인의 차이
- 일반 조인 실행시 연관된 엔티티를 함께 조회하지 않는다.
- JPQL은 결과를 반환할 때 연관관계를 고려하지 않고 단지 SELECT 절에 지정한 엔티티만을 조회한다.
- 페치 조인을 사용할 때만 연관된 엔티티도 함께 조회한다 (즉시 로딩)
- 페치 조인은 객체 그래프를 SQL 한번에 조회하는 개념이다.
ex) `select t from Team t join t.members m where t.name = '팀A'`
=> `SELECT T.* FROM TEAM T INNER JOIN MEMBER M ON T.ID=M.TEAM_ID WHERE T.NAME='팀A'`
여기서는 팀 엔티티만 조회하고, 회원 엔티티는 조회하지 않는다.

# 페치 조인의 특징과 한계
- 페치 조인 대상에는 별칭을 줄 수 없다.
	- ex) `select t from Team t join fetch t.members as m`
		  위 예시에서 페치 조인 대상에 m이라는 별칭을 부여하고 있다.
	- 하이버네이트는 가능하지만 가급적 사용하지 않는다.
- 둘 이상의 컬렉션은 페치 조인 할 수 없다.
- 컬렉션을 페치 조인하면 [[페이징 API]]를 사용할 수 없다.
	- 일대일 다대일 같은 단일 값 연관 필드들은 페치 조인해도 페이징이 가능하다.
	- 하이버네이트는 경고 로그를 나믹고 메모리에서 페이징을 한다 (매우 위험하다)