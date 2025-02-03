# 트랜잭션의 시작
트랜잭션은 하나의 Connection을 가져와 사용하다가 닫는 사이에서 일어난다. 
트랜잭션의 시작과 종료는 Connection 객체를 통해 이루어지기 때문이다.
# 트랜잭션의 종료
하나의 트랜잭션이 시작지면 commit() 또는 rollback() 호출될 때 까지가 하나의 트랜잭션으로 묶인다.

```java
public void executeQuery() throws SQLException {
    TransactionStatus status = transactionManager.getTransaction(new DefaultTransactionDefinition());
    // 트랜잭션 시작
    
    try {
        // 쿼리 실행
        ...
            
        transactionManager.commit(status);
    } catch (Exception e) { // Exception이 발생하면 rollback한다.
        transactionManager.rollback(status);
    }
}
```
# 트랜잭션 전파 속성
Spring이 제공하는 `@Transactional`의 장점 중 하나는 여러 트랜잭션을 묶어서 커다란 하나의 트랜잭션 경계를 만들 수 있다는 점이다. 작업을 하다보면 기존에 트랜잭션이 진행중일 때 추가적인 트랜잭션을 진행해야 하는 경우가 있다. 이미 트랜잭션이 진행중일 때 추가 트랜잭션 진행을 어떻게 할지 결정하는 것이 전파 속성(Propagation)이다.
![](https://blog.kakaocdn.net/dn/8DLel/btrIMBi5Mei/zG8FJfIn6FcPpSCbKsJrg1/img.png)

트랜잭션은 데이터베이스에서 제공하는 기술이므로 커넥션 객체를 통해 처리한다. 그래서 1개의 트랜잭션을 사용한다는 것은 하나의 커넥션 객체를 사용한다는 것이고, 실제 데이터베이스의 트랜잭션을 사용한다는 점에서 물리 트랜잭션이라고도 한다.
![](https://blog.kakaocdn.net/dn/s76hS/btrII0RHWG6/7EEuYI1P8Q99SDpXEQQZUK/img.png)

- 물리 트랜잭션: 실제 데이터베이스에 적용되는 트랜잭션으로, 커넥션을 통해 커밋/롤백하는 단위
- 논리 트랜잭션: 스프링이 트랜잭션 매니저를 통해 트랜잭션을 처리하는 단위

기존의 트랜잭션이 진행중일 때 또 다른 트랜잭션이 사용되면 복잡한 상황이 발생한다. 스프링은 논리 트랜잭션이라는 개념을 도입함으로써 상황에 대한 설명을 쉽게 만들고, 다음과 같은 단순한 원칙을 세울수 있었다.

- 모든 논리 트랜잭션이 커밋되어야 물리 트랜잭션이 커밋됨
- 하나의 논리 트랜잭션이라도 롤백되면 물리 트랜잭션은 롤백됨

# 스프링의 트랜잭션 전파 속성
## REQUIRED (DEFAULT)
![](https://blog.kakaocdn.net/dn/s76hS/btrII0RHWG6/7EEuYI1P8Q99SDpXEQQZUK/img.png)
여기서 참여한다는 것은 외부 트랜잭션을 그대로 이어간다는 뜻이며, 외부 트랜잭션의 범위가 내부까자 확장되는 것이다. 그러므로 내부 트랜잭션은 새로운 물리 트랜잭션을 사용하지 않는다.

하지만 트랜잭션 매니저에 의해 관리되는 논리 트랜잭션이 존재하므로 커밋은 내부 1회, 외부 1회해서 총 2회 실행된다. 물론 내부 트랜잭션은 논리 트랜잭션이기 때문에 커밋을 호출해도 즉시 커밋되지는 않고, 외부 트랜잭션이 최종적으로 커밋될 때 실제로 커밋이 된다. 롤백 역시 비슷한데, 내부 트랜잭션에서 롤백을 하여도 즉시 롤백되지 않는다. 물리 트랜잭션이 롤백될 때 실제 롤백이 처리되는데, 논리 트랜잭션들 중에서 1개라도 롤백되었다면 롤백된다. 물리 트랜잭션은 실제 커넥션에 롤백/커밋을 호출하는 것이므로 해당 트랜잭션이 끝나는 것이다.
## REQUIRES_NEW
REQUIRES_NEW는 외부 트랜잭션과 내부 트랜잭션을 완전히 분리하는 전파 속성이다. 그래서 2개의 물리 트랜잭션이 사용되며, 각각 트랜잭션 별로 커밋과 롤백이 수행된다.
![](https://blog.kakaocdn.net/dn/bfC5ox/btrII2aR0kb/ApEk8qjP9bzJBLWcR39Un0/img.png)
두 개는 서로 다른 물리 트랜잭션이므로, 내부 트랜잭션 롤백이 외부 트랜잭션 롤백에 영향을 주지 않는다. 그러므로 내부 트랜잭션이 롤백 호출은 실제 커넥션에 롤백을 호출하는 것이므로 트랜잭션이 끝나게 된다.

서로 다른 물리 트랜잭션을 별도로 가진다는 것은 각각의 디비 커넥션이 사용된다는 것이다. 즉, 1개의 HTTP 요청에 대해 2개의 커넥션이 사용되는 것이다. 내부 트랜잭션이 처리 중일때는 꺼내진 외부 트랜잭션이 대기하는데, 이는 데이터베이스 커넥션을 고갈시킬 수 있다. 그러므로 조심해서 사용해야 하며, 만약 REQURES_NEW 없이 해결 가능하다면 대안책(별도의 클래스를 두기 등)을 사용하는 것이 좋다.

![](https://blog.kakaocdn.net/dn/bHIrEE/btrILdW6dPy/xCwRpCG9RzXDlhfdJmjOY0/img.png)