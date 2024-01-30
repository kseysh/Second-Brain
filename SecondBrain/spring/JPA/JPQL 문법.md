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
`[NOT] EXISTS (subquery)` : 서브쿼리에 결과가 존재하면 참
ex) `select m from Member m where exists (select t from m.team t where t.name = '팀A')` : 팀 A 소속인 회워

`{ALL|ANY|SOME} (subquery)` : 
- ALL: 모두 만족하면 참
- ANY, SOME: 같은 의미, 조건을 하나라도 만족하면 참
`[NOT] IN (subquery)` : 서브쿼리의 결과 중 하나라도 같은 것이 있으면 