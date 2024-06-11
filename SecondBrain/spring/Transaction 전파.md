# 트랜잭션의 시작
트랜잭션은 하나의 Connection을 가져와 사용하다가 닫는 사이에서 일어난다. 
트랜잭션의 시작과 종료는 Connection 객체를 통해 이루어지기 때문이다.
# 트랜잭션의 종료
하나의 트랜잭션이 시작지면 commit() 또는 rollback() 호출될 때 까지가 하나의 트랜잭션으로 묶인다.

```
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
#