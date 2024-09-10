
|             |                                     |                                         |
| ----------- | ----------------------------------- | --------------------------------------- |
|             | Redis                               | Memcached                               |
| **저장소**     | In Memory Storage                   | In Memory Storage                       |
| **저장 방식**   | Key-Value                           | Key-Value                               |
| **데이터 타입**  | String, Set, Sorted Set, Hash, List | String                                  |
| **데이터 저장**  | Memory, Disk                        | Only Memory                             |
| **메모리 재사용** | 메모리 재사용 하지 않음(명시적으로만 데이터 삭제 가능)     | 메모리 부족시 LRU 알고리즘을 이용하여 데이터 삭제 후 메모리 재사용 |
| **스레드**     | **Single Thread**                   | **Multi Thread**                        |
| **캐싱 용량**   | Key, Value 모두 512MB                 | Key name 250 byte,  Value 1MB           |
### 비교하여 Redis의 장점
- 다양한 자료구조 지원
- 데이터 복구가 가능
	- 데이터를 Disk에도 저장하기 때문
- 다양한 Data Eviction(공간이 필요할 때, 어떤 데이터를 지우는 것) 정책을 지원합니다.
	- Memcached는 LRU 알고리즘을 통한 Eviction을 지원한다. 
	- 하지만 Redis는 6가지 Eviction정책을 통해 더욱 세밀한 Eviction 제어가 가능하다.

### 비교하여 Memcached의 장점
- 멀티 스레드 아키텍처 지원
	- 서버 scale up에 유리하다
- Redis에 비해 적은 메모리를 요구
	- Redis는 Copy&Write 방식을 사용하기 때문에 실제 사용하는 메모리보다 더 많은 메모리를 요구

## Spring이 Memcached 보다 Redis를 사용하는 이유
Redis는 Memcached의 단점을 개선하여 만듦. 
Spring 운영 서버에서 데이터 복구 기능과 다양한 자료구조를 가진 Redis는 캐싱과 세션 관리에 있어서 더욱 강력한 기능을 제공함.

그렇기 때문에 메모리 파편화와 약간의 속도 차이를 감수하고도 Memcached에 비해 강력한 운영 기능을 가진 Redis를 Spring은 선택.