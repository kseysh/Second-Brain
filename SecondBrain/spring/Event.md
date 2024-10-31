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

## Event를 사용해도 Transaction이 다 같이 rollback될 수 있을까요?
순간 그런 생각이 들었어요 Event를 발행만하고 이후의 이벤트 구독자의 행위를 관리하지 않는다면 제대로된 Transaction rollback이 일어나지 않을 수도 있지 않을까? 그래서 Event와 Transaction에 대해서 좀 더 알아보게 되었습니다.
### 동기 이벤트 처리
스프링의 기본 이벤트 발행 방식은 동기적으로 동작합니다. 동기 방식으로 동작하는 것이 중요한 이유는 트랜잭션이 하나의 범위로 묶일 수 있기 때문입니다. 만약 이벤트를 발행하는 곳에서 트랜잭션이 시작된 상태라면 이벤트를 구독하는 곳에서도 동일한 트랜잭션을 공유하게 됩니다.
이벤트 처리가 하나의 트랜잭션의 안에서 실행된다면 트랜잭션의 커밋 전/후, 완료 또는 롤백에 따라 동작을 고려해야할 수 있습니다. 이는 스프링 4.2이후부터 지원하는 `@TransactionEventListener`를 이용하여 여러 옵션을 이용해 트랜잭션 단계에 관여할 수 있습니다.

#### 예시
이벤트를 발행한 서비스 메서드와 이벤트를 구독하는 서비스 메서드가 모두 같은 트랜잭션 컨텍스트 안에 있게 되므로, 구독하는 서비스에서 발생한 예외가 전파되어 트랜잭션이 롤백됩니다.
```java
public void publishEvent() {
    applicationEventPublisher.publishEvent(new MyEvent(this));
}
```
```java
@Component
public class MyEventListener {
    @EventListener
    @Transactional
    public void handleEvent(MyEvent event) {
        // 구독 서비스에서 예외 발생 시 트랜잭션 롤백
        throw new RuntimeException("오류 발생으로 인한 롤백");
    }
}
```

#### `@TransactionalEventListener`와 트랜잭션 롤백
`@TransactionalEventListener`를 사용하면 트랜잭션의 특정 상태에서 이벤트 리스너가 실행되도록 설정할 수 있습니다. `AFTER_COMMIT` (커밋 후)나 `BEFORE_COMMIT`(커밋 전) 등으로 이벤트 실행 시점을 설정할 수 있지만, `AFTER_COMMIT`으로 설정한 경우 이벤트는 이미 커밋된 이후에 실행되므로 이 시점에서 발생한 오류는 트랜잭션에 영향을 주지 않습니다. 반면, `BEFORE_COMMIT`으로 설정하면 트랜잭션이 커밋되기 전에 이벤트가 실행되므로, 이 경우 예외 발생 시 트랜잭션이 롤백될 수 있습니다.
##### 예시
```java
@TransactionalEventListener(phase = TransactionPhase.BEFORE_COMMIT)
public void handleEvent(MyEvent event) {
    throw new RuntimeException("트랜잭션 롤백");
}
```

반대로 비동기 이벤트 처리를 하게 되면 이벤트 리스너는 트랜잭션과 별도로 실행되고 이벤트 구독자가 별도의 스레드에서 실행되므로, 비동기적으로 동작하는 이벤트 리스너에서 발생한 예외는 이벤트 발행 메서드의 트랜잭션에 영향을 미치지 않습니다.
이를 이용하여 이벤트를 발행만하고 이벤트 구독자가 행하는 것에 영향을 받고 싶지 않다면 비동기 이벤트 처리를 고민하는 것도 좋은 선택으로 보여집니다.
이벤트를 비동기로 처리하기 위해서는 아래의 두 가지 방법을 사용할 수 있습니다.
- `@Async` 메소드로 비동기 구현 -> 일부 메시지만 비동기 처리할 경우
- `ApplicationEventMulticaster`로 비동기 구현 -> 모든 메시지를 기본적으로 비동기 처리할 경우

## 이벤트 사용의 장점과 단점
이벤트 사용을 하면 이렇게 의존성을 분리할 수 있다는 장점이 있지만, 한 기능에는 무조건적인 장점이 있지도, 무조건적인 단점이 있지도 않다고 생각해요. 이벤트는 어떤 단점을 가지고 있을까요?
### - 장점
- *의존성을 분리하여 두 클래스를 느슨하게 결합시킬 수 있음*
- 클래스가 독립적이므로 재사용성을 높일 수 있음
- 추후에 별도의 서비스로 분리하기 용이함
- 메세지 구독 모듈을 추가 또는 삭제할 때, 다른 모듈에 영향을 주지 않은 채로 수정할 수 있음
- 단위 테스트가 용이해짐
### 단점
- 전반적인 작업량이 많아질 수 있음(이벤트 클래스, 커스텀 어노테이션 등)
- 코드 흐름을 따라가기 어려움
- 메세지 구독 순서를 고려해야 하는 경우 복잡해짐
- 전체적인 이벤트의 구독 및 발행 과정을 테스트하기 어려움
- 특정 프레임워크 API에 의존하게 됨

## 이벤트를 사용해야 하는 경우
특정한 도메인의 상태 변경을 외부로 알려주어야 하는 경우에 사용합니다.