## `query.getResultList()`
결과가 하나 이상일 때, 리스트 반환
결과가 없으면 빈 리스트 반환
## `query.getSingleResult()`
결과가 정확히 하나, 단일 객체 반환
결과가 없으면 `NoResultException` 반환
둘 이상이면 `NonUniqueResultException` 반환