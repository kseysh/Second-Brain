반환 타입이 명확하지 않을 때 사용한다.
반환 타입이 명확할 때 => [[TypeQuery]]

```
Query query = em.createQuery("SELECT m.username, m.age from Member m")
```
위 예제에서 username과 age는 서로 타입이 달라 반환 타입이 명확하지 못하다 따라서 Query 객체를 사용한다.