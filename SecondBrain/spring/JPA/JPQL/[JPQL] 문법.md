# 조인
### 내부 조인
`select m from Member m join m.team t`
### 외부 조인
`select m from Member m left join m.team t`
### 세타 조인
`select count(m) from Member m, Team t where m.username = t.name`

## 조인 대상 필터링
`select m, t from Member m left join m.team t on t.name = 'A'`
=> SQL문으로 `select m.*, t.* from Member m left join Team t on m.TEAM_ID=t.id and t.name='A'`

## 연관 관계 없는 엔티티 외부 조인
`select m, t from Member m left join m.team t on m.username = t.name`
=> SQL문으로 `select m.*, t.* from Member m left join Team t on m.USER_ID = t.name`

# 서브 쿼리
### 나이가 평균보다 많은 회원
`select m from Member m where m.age > (select avg(m2.age) from Member m2)`
### 한 건이라도 주문한 고객
`select m from Member m where (select count(o) from Order o where m = o.member) > 0`

## 서브 쿼리 지원 함수
#### `[NOT] EXISTS (subquery)` : 서브쿼리에 결과가 존재하면 참
ex) 팀 A 소속인 회원
`select m from Member m where exists (select t from m.team t where t.name = '팀A')`

#### `{ALL|ANY|SOME} (subquery)`  
- ALL: 모두 만족하면 참
- ANY, SOME: 같은 의미, 조건을 하나라도 만족하면 참
ex1) 전체 상품 각각의 재고보다 주문량이 많은 주문들
`select o from Order o where o.orderAmount > ALL (select p.stockAmount from Product p)`

ex2) 어떤 팀이든 팀에 소속된 회원
`select m from Member m where m.team = ANY (select t from Team t)`

`[NOT] IN (subquery)` : 서브쿼리의 결과 중 하나라도 같은 것이 있으면 참

### JPA 서브 쿼리의 한계
JPA는 WHERE, HAVING 절에서만 서브 쿼리 사용 가능
하이버네이트가 지원한다면 SELECT 절도 가능하다.
FROM 절의 서브 쿼리는 현재 JPQL에서 불가능하다. (하이버네이트6 부터는 FROM절의 서브쿼리를 지원한다.)
- JOIN으로 풀 수 있으면 풀어서 해결하는 것이 좋다.

# JPQL 타입 표현
- 문자 : 'HELLO'
- 숫자 : 10L(Long), 10D(Double), 10F(Float)
- Boolean : TRUE, FALSE
- ENUM : jpabook.MemberType.Admin(패키지명 포함)
	- ex) `select m.username, 'HELLO', true from Member m where m.type = jpql.MemberType.USER`
- 엔티티 타입: TYPE(m) = Member (상속 관계에서 사용)
	- ex) `select i from Item i where type(i) = Book`
이외의 표준 sql 문법들은 대부분 지원한다.

# 조건식
## CASE 식
![[Pasted image 20240130171029.png]]
![[Pasted image 20240130171040.png]]

# JPQL 기본 함수
- CONCAT
- SUBSTRING
- TRIM
- LOWER, UPPER
- LENGTH
- LOCATE
- ABS, SQRT, MOD
- SIZE, INDEX
### JPQL에서 사용자 정의 함수 호출
하이버네이트는 사용전 방언에 추가해야 한다.
사용하는 DB 방언을 상속받고, 사용자 정의 함수를 등록한다.
ex) `SELECT FUNCTION('group_concat', i.name) from Item i`