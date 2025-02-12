## Write Lock (exclusive Lock)
write할 때 사용한다.
다른 트랜잭션의 write와 read를 모두 막는다.
## Read Lock (Share Lock)
Read할 때 사용한다.
다른 트랜잭션의 write만을 막는다.

## Lock을 사용했을 때 Serializable하지 않은 예제
### serial schedule
![[Pasted image 20250212161211.png|400]]
![[Pasted image 20250212161237.png|400]]
### nonserializable한 nonserinal schedule
![[Pasted image 20250212161400.png|400]]
