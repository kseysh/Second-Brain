JPA는 페이징을 두 API로 추상화한다.
`setFirstResult(int startPosition)` : 조회 시작 위치
`setMaxResults(int maxResult)` : 조회할 데이터 수

ex)
```
List<Member> result = em.createQuery("select m from Member m order by m.age desc", Member.class)
	.setFirstResult(0)
	.setMaxResult(10)
	.getResultList();
```