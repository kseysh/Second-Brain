## Redis의 특징
- 데이터 타입은 String, Set, List, Sorted set, Hash, Bit arr, HyperLog, Stream을 지원한다.
- 데이터 저장은 메모리를 포함하여 디스크에도 저장을 할 수 있다.
- 스냅샷을 통해서 디스크에 담을 수 있는데 이를 통해서 비휘발성의 특징도 가지고 있다.
    - 반복적인 스냅샷을 통하여 디스크에 저장해 메모리의 여유 공간을 만들 수 있다.
- 싱글쓰레드를 지원한다.
- 캐시 용량은 Key와 Value 모두 512MB를 지원한다.
- 데이터를 복제하고, 들어온 데이터를 실시간 업데이트가 가능하여 서버 복제가 가능하다.
    - 이러한 이유로 스토리지로도 사용할 수 있다.
- ACID를 유사하게 지원해서 트랜잭션을 걸 수 있다.

### Single thread여서 발생하는 문제점
![[Pasted image 20240910235147.png]]
KEYS, FLUSHALL, FLUSHDB, Delete Collections, Get All Collections와 같은 시간 복잡도가 O(n)인 명령어의 경우에는 해당 명령이 처리될 때까지 다음 명령어들이 대기 상태로 전환되는 문제가 발생한다.
=> Redis가 성능 저하의 원인이 될 수도 있다
=> 하지만 Race Condition에서 유리하다.

### 메모리 관리
메모리의 특성상 메모리 단편화가 발생한다.
따라서, 7KB를 사용하지만 공간은 10KB가 남아있는 상황이 발생할 수 있다.
따라서 실제 물리 메모리 사용량을 모니터링 하며 메모리 관리를 해주어야 한다.
![[Pasted image 20240910235436.png]]

### Redis의 목적성
Persistence 기능(메모리에 저장된 데이터를 디스크에 영속화하는 기능)은 장애 발생 가능성이 높다.
따라서 데이터가 유실되어도 치명적인 문제가 없다면, Persistence 기능을 제거하는 것이 좋다.
![[Pasted image 20240910235620.png]]

## redis.conf 권장 설정
Maxclient 설정 50000
RDB/AOF 설정 off
특정 commands disable
- keys
적절한 ziplist 설정