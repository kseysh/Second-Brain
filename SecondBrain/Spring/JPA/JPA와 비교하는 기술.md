
### JPQL
```java
// 문자열이라 오타 나면 런타임 에러 발생
em.createQuery("select m from Member m where m.username = :username", Member.class)
    .setParameter("username", "kim")
    .getResultList();
```

### JPA Criteria API
JPA 표준 빌더로 가독성이 좋지 않음 (타입 체크는 가능)
```java
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery<Member> cq = cb.createQuery(Member.class);
Root<Member> m = cq.from(Member.class);

// 너무 장황함
cq.select(m).where(cb.equal(m.get("username"), "kim"));
em.createQuery(cq).getResultList();
```
### QueryDSL
JPA 확장 빌더로 동적 쿼리를 지원하며 타입 체크를 지원함
가독성이 좋음
```java
// 직관적이고 자동완성 지원
queryFactory
    .selectFrom(member)
    .where(member.username.eq("kim"))
    .fetch();
```
### MyBatis
특징: SQL 중심으로 개발하여 XML로 매핑함
```xml
<select id="findByName" resultType="Member">
    SELECT * FROM MEMBER WHERE USERNAME = #{username}
</select>
```
JPA로 해결하기 어려운 통계성 쿼리를 써야할 때만 부분적으로 MyBatis나 JdbcTemplate 사용