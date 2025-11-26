Reactive Programming은 기본적으로 비동기로 실행된다.
하지만, 비동기로 실행되면서 Blocking IO를 만난다면, 스레드가 Block되며 심각한 성능 문제를 겪을 수 있다. 
따라서 Schedular 인터페이스를 활용하여 Blocking인 로직을 Non-Blocking 흐름으로 안전하게 처리하기 위해 사용할 수 있습니다.


#### 예제 코드
```java
Mono.fromCallable(() -> chatRoomService.canUserAccessRoom(roomId, userId))
                .subscribeOn(Schedulers.boundedElastic())
```
DB 조회 같은 블로킹 작업을 별도의 전용 스레드에서 실행하여, 메인 서버 스레드가 멈추지 않도록 한다는 의미입니다.

###### Mono.fromCallable(() -> ...)
동기적으로 결과를 반환하는 일반 메서드(chatRoomService.canUserAccessRoom)를 리액티브 타입인 Mono로 감싸준다.
######  subscribeOn(...)
Mono의 데이터 생성이 어떤 스레드에서 이루어질지 결정
###### `Schedulers.boundedElastic()`
Project Reactor에서 제공하는 스레드 풀 중 하나로 Blocking I/O 작업에 최적화되어 있다.
필요에 따라 스레드를 생성하지만, 무한정 늘어나지 않도록 제한(Bound)을 두어 시스템 자원을 보호한다
작업이 끝나면 스레드를 유휴 상태로 두었다가 재사용합니다(Elastic).

## 사용해야 하는 이유
WebFlux는 소수의 이벤트 루프 스레드로 모든 요청을 처리한다.
만약 이렇게 이벤트 루프 코드에 Blocking 로직을 위임하지 않으면 Blocking IO가 스레드를 Block하는 동안 서버의 메인 스레드 하나가 그 동안 멈출 수 있는 문제가 발생한다.
따라서 Blocking IO를 별도의 작업자 스레드로 던져 메인 이벤트 루프 스레드가 다른 사용자의 요청을 계속 처리하도록 하고 작업이 끝나면 결과만 비동기적으로 받아온다.