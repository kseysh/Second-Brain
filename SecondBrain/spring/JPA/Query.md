반환 타입이 명확하지 않을 때 사용한다.
반환 타입이 명확할 때 => [[TypeQuery]]

```
Query query = em.createQuery("SELECT m.username, m.age from Member m")
```
username과 age