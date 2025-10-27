Outer method가 Transactional이 아니고, Inner method가 Transactional일 때 트랜잭션 적용이 안되는 문제

@Transactional의 동작원리 => 프록시 객체 활용
@Transactional이 붙은 클래스에 대해 프록시 객체를 생성하여 트랜잭션 기능을 주입한다.

같은 클래스 내부의 메서드에서 @Transactional이 붙은 다른 메서드를 호출할 때는 this를 통한 직접 호출 발생
이 경우 프록시를 거치지 않고 실제 객체의 메서드가 바로 실행되어 트랜잭션이 적용되지 않는다.

### 해결 방식
@Transactional 메서드를 별도의 Bean으로 분리