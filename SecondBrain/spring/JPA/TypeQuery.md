반환 타입이 명확할 때 사용한다.
반환 타입이 명확하지 않을 때 => [[Query]]

ex)
```
TypedQuery<Member> query = em.createQuery("SELECT m FROM Member m", Member.class)
```

이후 
`List<Member> resultList = query.getResultList();`
나
`Member singleResult = query.getSingleResult()`
로 값을 가져올 수 있다.