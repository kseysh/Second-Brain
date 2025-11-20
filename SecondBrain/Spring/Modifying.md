@Query 어노테이션으로 작성된 native query에서 변경이 필요한 쿼리에 사용되는 어노테이션

## 주의점
@Query 어노테이션으로 작성된 JPQL/native query는 영속성 컨텍스트를 거치지 않고 DB에 값을 바로 읽기/반영한다.
따라서 Modifying 어노테이션 옵션 중 **clearAutomatically = true** (default는 false)를 해주면 쿼리 실행마다 캐시를 비울 수 있다.