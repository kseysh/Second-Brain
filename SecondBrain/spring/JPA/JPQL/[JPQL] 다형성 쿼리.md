# TYPE
조회 대상을 특정 자식으로 한정한다.

\[JPQL] : `select i from Item i where type(i) IN (Book, Movie)`
\[SQL] : `select i from i where i.DTYPE in ('B', 'M')`

# TREAT (JPA 2.1)
- 자바의 타입 캐스팅과 유사
- 상속 구조에서 부모 타입을 특정 자식 타입으로 다룰 때 사용한다.
- FROM, WHERE, SELECT절에서 사용
ex) 
\[JPQL] : `select i from Item i where treat(i as Book).auther = 'kim'`
\[SQL] : `select i.* from Item i where i.DTYPE = 'B' and i.auther = 'kim')`
