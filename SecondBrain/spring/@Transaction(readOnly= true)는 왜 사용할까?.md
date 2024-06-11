# 변경 감지 (dirty checking)
JPA는 영속성 컨텍스트가 수행하는 변경감지를 진행하는데, 영속성 컨텍스트는 Entity 조회 시 초기 상태에 대한 Snapshot을 저장한다. 트랜잭션이 Commit될 때 , 초기 상태의 정보를 가지는 Snapshot과 Entity의 상태를 비교하여 변경된 내용에 대해 update query를 생성해 쓰기 지연 저장소에 저장한다.

그 후, 일괄적으로 쓰기 지연 저장소에 저장되어 있는 SQL query를 flush 하고 데이터베이스의 트랜잭션을 Commit 함으로써 우리가 update와 같은 메서드를 사용하지 않고도 Entity의 수정이 이루어진다. 이를 변경 감지(Dirty Checking) 라고 한다.

# readonly = true의 장점
`readonly = true`를 설정하게 되면 스프링 프레임 워크는 JPA의 세션 플러시 모드를 MANUAL(트랜잭션 내에서 사용자가 수동으로 flush를 호출하지 않으면 flush가 자동으로 수행되지 않는 모드)로 설정한다.
즉, 트랜잭션 내에서 강제로 `flush()`를 호출하지 않는 한, 수정 내역에 대해 DB에 적용되지 않는다.
이로 인해 트랜잭션 Commit시 영속성 컨텍스
