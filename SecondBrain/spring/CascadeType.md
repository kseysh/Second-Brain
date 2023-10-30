## CascadeType이란?
JPA에서 관계형 데이터베이스와의 연관성을 다룰 때 사용되는 어노테이션이다. CascadeType을 통해 엔티티 간의 연관성을 관리할 때 어떤 동작이 연관된 엔티티에 전파되어야 하는지를 지정할 수 있다.
CascadeType으로 부모 엔티티에서 자식 엔티티로 연관된 동작들을 자동으로 전파할 수 있다.

### CascadeType의 종류
1. `CascadeType.ALL`: 모든 연관 동작을 Cascade한다. 부모 엔티티에 대한 모든 변경이 자식 엔티티로 전파된다.
2. `CascadeType.PERSIST`: 저장 연산을 Cascade한다. 즉, 부모 엔티티를 저장할 때 자식 엔티티도 함께 저장된다
3. `CascadeType.MERGE`: 업데이트 연산을 Cascade한다. 부모 엔티티를 업데이트하면 자식 엔티티도 함께 업데이트된다.
4. `CascadeType.REMOVE`: 삭제 연산을 Cascade한다. 부모 엔티티를 삭제하면 자식 엔티티도 함께 삭제된다.
5. `CascadeType.REFRESH`: 엔티티를 새로 고침 연산을 Cascade한다. 부모 엔티티를 새로 고침하면 자식 엔티티도 함께 새로 고침된다.
6. `CascadeType.DETACH`: 엔티티를 분리 연산을 Cascade한다. 부모 엔티티를 분리하면 자식 엔티티도 함께 분리된다.
7. `CascadeType.PERSIST` 및 `CascadeType.MERGE` 등 여러 값들을 조합하여 원하는 동작을 정의할 수 있다

