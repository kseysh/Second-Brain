## 왜 이 기술을 선택했는지?
DB에서 모든 데이터를 가져오고, 이를 다시 sorting하여 api를 반환하는 과정이 너무 오래 걸린다고 생각했기 때문.
n명의 유저가 있을 때, n명의 데이터를 가져오고, 이를 logn의 시간 복잡도로 정렬하고 다시 이를 wrapping해서 반환하는 것이 오래 걸릴 것으로 판단
따라서 redis의 sorted set을 이용해서 스탬프 등록시에 점수를 1 증가시키고 유저 DB에서 또 증가시키는 방식으로 구현

또한 다른 요소를 추가하여 랭킹을 유연하게 구성할 수 있는 장점이 있다.
## 다른 대안은 없는지?
SQL에서 직접 order_by를 통해 값을 가져온다 => 가져올 때 마다 시간이 너무 오래걸린다.

DB를 이용해 계산한 랭킹 값을 redis에 저장해두고, 일정 기간마다 캐시를 업데이트 한다. (하지만, 솝탬프의 랭킹은 경쟁을 유도하기 위해 실시간으로 갱신되는 것이 좋기 때문에 sorted set을 사용하는 것이 더 좋을 것으로 판단된다.)
=> 이 방식은 한 마디 관리에 사용하자!
## 이 기술의 단점
모든 유저의 정보가 들어간다면 메모리가 충분히 많이 필요할 수도 있을 것 같다.
메모리가 만약 가득 찬다면? 
=> redis에서 캐싱한 다른 데이터에 데이터가 일정 이상 초과하면 삭제 처리를 하도록 한다 (이 정보는 삭제를 할 수는 없다 삭제하더라도 바로 복구되기 때문)
만약 Makers가 아니고 유저가 많다면 일정 수로만 유지하고 랭킹에 들어가지 않는 유저는 캐싱에서 제외하는 방식으로 해야할듯

스탬프 삭제 / 추가 / 수정시에 항상 redis도 같이 수정해줘야 한다는 단점이 있다 => 이는 캡슐화를 통해 수정이 가능할 것 같다.
만약, redis가 분산되어 있다면 정합성이 안 맞는 시기가 있을 수 있다.
## 적용 전 성능
랭킹 조회
![[Pasted image 20250113150118.png]]
![[Pasted image 20250113222829.png]]
![[Pasted image 20250113222817.png]]

## 적용 후 성능
![[Pasted image 20250210201652.png]]
![[Pasted image 20250210201702.png]]
swap 메모리를 사용하기 때문에 성능 측정이 예상보다 적게 측정될 수 있다.
## 실무에서도 적용 가능한가?
실무에서는 Makers와 달리 유저의 수가 다르므로 적절한 인원만 랭킹에 진입할 수 있도록 하는 것이 중요할 것 같다. (메모리를 너무 많이 잡아먹지 않도록 주의해야겠다)

## 예상되는 문제점
DB에서 데이터를 가져오고 redis에 적용하는 순간에 스탬프에 추가/삭제가 있을 경우 그 정보를 놓칠 수 있다.
 => Redis Pub/Sub을 이용해서 메시징 시스템을 구축한다면 이를 해결할 수 있다.
 ![[Pasted image 20250119142746.png|300]]
2. redis watch를 이용한다 (redis의 트랜잭션)
	1. 하지만 이는 레디스 클러스터 환경에서 제한적으로 사용할 수 없다.
### 해결방법
redis에 데이터가 다 적용이 된 이후에 DB 락을 해제해준다.
## 정합성은 맞는가?
랭킹 데이터를 가져오는 중에는 스탬프를 찍는 행위를 지연한다면, 정합성이 맞을 것으로 판단된다.
## 오류 발생 시 대처는 어떻게 되는가?
오류 발생 시 기존 방식처럼 DB에서 값을 가져와서 반환할 수 있도록 구현한다.
### 오류 체킹은?
새로 랭킹 값을 가져올시에 로그를 찍는다.
## 트러블 슈팅
### virtual thread 관련 문제
=> 부하 테스트 시에 부하가 발생한 후 계속 504 Timeout이 발생하는 문제 발생, 문제 발생 이후에는 SIGTERM도 작동하지 않음
스레드 덤프 분석해보니 ForkJoinPool-1-worker라는 이름의 스레드의 상태가 UNABLE_DETERMINE임을 확인
이는 가상 스레드에서 상태를 확인할 수 없을 때 만들어지며, 정상 상태일 때는 RUNNABLE인 것을 확인
따라서 가상 스레드에 부하를 가했을 때 생기는 문제라고 판단하여 virtual thread의 전역 사용을 중지함
그러니 기존보다 더 많은 수의 요청을 받을 수 있게 되며 문제를 해결할 수 있었음
#### 문제의 원인
예상이지만, 가상 스레드를 사용하고 가상 스레드는 랭킹 작업이 정렬을 포함해서 cpu 사용률이 높았고, 
뭘까..

## Redis의 Set이 순서를 보장하는 이유
`Set<TypedTuple<Long>>`을 getClass 해보았을 때, Set의 구현체가 LinkedHashSet이기 때문이다.

# 원래 버전
![[Pasted image 20250228212015.png]]
CPU 100퍼 찍으면서 260 TPS 기록
DB 병목은 발생하지 않음

![[Pasted image 20250228230739.png|400]]
## redis sorted set 버전
![[Pasted image 20250228225325.png|400]]
1. redis로 id를 저장한 랭킹 저장
2. id를 이용해 이름, 파트, 한마디 캐싱
	1. 캐싱된 값이 없는 엔티티가 있다면 전체 캐싱 다시
3. id를 이용해 두 값을 맞추고, 반환
## index 버전

데이터 100만개 넣고 다시 시도해봅시다!!!
표본의 개수가 너무 적음, generation의 기수성이 너무 낮아 인덱스를 타지 않는다.
옵티마이저가 seq scan이 더 낫다고 판단하고 seq scan을 해요
=> clustering index와 secondary index의 차이 때문

```sql
explain analyze  
    select   
	    su1_0.generation,  
        su1_0.nickname,  
        su1_0.profile_message,  
        su1_0.total_points  
    from  
        app_prod.soptamp_user su1_0  
    where  
        su1_0.generation=35  
    order by su1_0.total_points desc;
```
#### 커버링 인덱스 시도
```sql
CREATE INDEX idx_for_rank  
ON app_prod.soptamp_user (generation, total_points DESC) INCLUDE (nickname, profile_message);
```
excution time이 줄어들었다! 0.140ms -> 0.110ms
하지만, `set enable_seqscan = off;`를 적용해줘야 함

기존 버전 : git checkout 3e27985
sorted set 버전: git checkout8cfeac1
index 버전:  git checkout e654c7e