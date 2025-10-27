JPA는 새로운 Entity이면 Persist가 실행이 되고, 새로운 entity가 아니면 merge를 실행한다.

하지만, 새로운 Entity인지 모르는 엔티티는 update할 객체일 수도 있으므로 select로 확인을 우선 하고, persist인지 merge인지 구별하는 방식이다.

나의 경우에는 Id를 snowflake Id 생성 방식으로 생성해주어 새로운 객체인지 이미 존재하는 객체인지 Jpa가 파악하지 못했고, 따라서 saveAll 함수를 호출하더라도 하나씩 select를 실행해보고 merge를 할지 persist를 할지 결정한 것이다.