JPQL에서 엔티티를 직접 사용하면 SQL에서 해당 엔티티의 기본 키 값을 사용한다.

ex)
\[JPQL] 
`select count(m.id) from Member m` //엔티티의 아이디를 사용 
`select count(m) from Member m` //엔티티를 직접 사용 

\[SQL](JPQL 둘다 같은 다음 SQL 실행) 
`select count(m.id) as cnt from Member m`

엔티티를 파라미터로 넘기거나 식별자를 파라미터로 전달해도 똑같이 SQL문에서는 해당 엔티티의 pk값을 사용한다.


