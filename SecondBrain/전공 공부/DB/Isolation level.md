## Isolation이 안될 때 나타날 수 있는 여러 현상
이 현상들을 모두 발생하지 않게 만들 수 있지만, 그러면 제약사항이 많아져서 동시 처리 가능한 트랜잭션 수가 줄어들어 결국 DB의 Throughput이 하락하게 된다.
### Dirty read
![[Pasted image 20250207165502.png|300]]
Commit되지 않은 변화를 읽음
이로 인해 commit 되기 전에 rollback이 되면 잘못된 값을 읽을 수도 있다는 문제가 생긴다.
#### abort를 수행하지 않고 Dirty read
![[Pasted image 20250207171428.png|300]]
x가 y에 40을 이체하는 것이므로 x와 y의 합은 항상 100이어야 하는데, Transaction 2에서는 합이 60이므로 잘못된 값을 읽고 있다. 이처럼 abort가 일어나지 않아도 Dirty read가 발생할 수 있다.
### Non-repeatable read (Fuzzy read)
![[Pasted image 20250207165330.png|300]]
같은 데이터의 값이 달라지는 현상
### phantom read
![[Pasted image 20250207165701.png|300]]
하나의 Transaction에서 같은 조건에서 두 번 읽었는데 결과가 달라졌으므로 이는 Isolation을 위반한 것

# Isolation level
일부 현상을 허용하는 몇 가지 level을 만들어 사용자가 필요에 따라서 적절하게 선택할 수 있도록 하는 것.
따라서 개발자는 isolation level을 통해 Throughput과 데이터 일관성 사이에서 어느정도 거래를 할 수 있다.
![[Pasted image 20250207170018.png|400]]
## SNAPSHOT Isolation
표준에서 정의한 Isolation level과는 약간 다르며, 

### Isolation level에서 정의되지 않았던 현상
#### Dirty write
commit 안된 데이터를 write함
![[Pasted image 20250207170323.png|400]]
Transaction 1은 10으로 바꾸고 abort를 하여 100이 되었다 쳐도, Transaction 2도 abort를 하게 되면 10으로 바꿔지게 되어 정합성 오류가 발생할 수 있다. 
게다가 Transaction 2가 만약 commit을 했다고 해도 Transaction 1이 abort를 하여 0으로 값이 바뀐다면 이는 Duration 속성도 만족하지 못하게 된다.
#### Lost update
업데이트를 덮어씀
![[Pasted image 20250207170822.png|400]]
#### Read skew
![[Pasted image 20250207172258.png|400]]
#### Write skew
![[Pasted image 20250207172454.png|400]]
