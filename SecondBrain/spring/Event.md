# Spring Event란?
탈퇴 service -> Application EventPublisher -> Schedule Event -> 데이터 삭제 Service
의 순서를 가지며, 탈퇴 service가 이벤트를 등록하면 EventPublisher가 Event를 발행하고 이 특정 Event를 구독하고 있던 Service들은 이벤트에 대한 로직을 실행하는 과정입니다.

이 Spring Event가 중요한 이유는 원래였다면 탈퇴 Service는 데이터 삭제 Service를 모두 DI 받아 사용해야 합니다.
그렇다면 데이터베이스에 userId 컬럼을 가지고 있는 엔티티의 모든 Service를 호출하게 되어 엄청난 의존성 결합이 발생하게 됩니다.
이를 탈퇴 Service는 탈퇴 Event를 발행하기만 하고, 데이터 삭제 Service들은 탈퇴 Event를 구독하여 탈퇴 Event가 발생했을 때, 자신에게 주어진 데이터 삭제 임무만을 맡을 수 있어 유저 탈퇴를 담당하는 객체가 다른 Service Layer와 의존성 결합을 이루지 않아도 됩니다.
이를 통해 개발자가 각각 분리된 코드를 작성할 수 있다는 장점이 생깁니다.

## Event의 단점
하지만 이벤트 발행은 트랜잭션 안에서 트랜잭션의 성공적인 수행을 보장할 수 없다는 문제가 발생하게 됩니다.
이를 위해서 스프링에서는 EventListener 대신 TransactionalEventListener를 제공하는데요, 말 그대로 트랜잭션 안에서 이벤트를 발생시킬 때 트랜잭션 처리와 결합하여 이벤트를 수신하는 로직을 처리할 수 있습니다.

