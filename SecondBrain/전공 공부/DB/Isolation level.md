## Isolation이 안될 때 나타날 수 있는 여러 현상
이 현상들을 모두 발생하지 않게 만들 수 있지만, 그러면 제약사항이 많아져서 동시 처리 가능한 트랜잭션 수가 줄어들어 결국 DB의 Throughput이 하락하게 된다.
### Dirty read
![[Pasted image 20250207165502.png|300]]
Commit되지 않은 변화를 읽음
이로 인해 commit 되기 전에 rollback이 되면 잘못된 값을 읽을 수도 있다는 문제가 생긴다.
### Non-repeatable read (Fuzzy read)
![[Pasted image 20250207165330.png|300]]
같은 데이터의 값이 달라지는 현상
### phantom read
![[Pasted image 20250207165701.png|300]]
하나의 Transaction에서 같은 조건에서 두 번 읽었는데 결과가 달라졌으므로 이는 Isolation을 위반한 것
# Isolation level
일부 현상을 허용하는 몇 가지 level을 만들어 사용자가 필요에 따라서 적절하게 선택할 수 있도록 하는 것.
![[Pasted image 20250207170018.png|400]]