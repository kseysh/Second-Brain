## 현재 prod 서버에서의 부하 테스트

스레드 수 100
Ramp-up 1
roop count 1
로 요청을 보낸 결과
![[Pasted image 20240624214419.png]]

## 캐싱 이후
![[Pasted image 20241119232357.png]]
초당 요청 수 100% 증가
평균 응답 시간 60% 감소
# 캐시 전략
캐싱 전략은 웹 서비스 환경에서 메모리를 사용해서 데이터베이스보다 훨씬 빠르게 데이터를 응답할 수 있어 이용자에게 빠르게 서비스를 제공할 수 있는 중요한 기술입니다.
하지만 메모리의 용량에는 한계가 있어 데이터를 모두 캐시에 저장해버리면 용량 부족 현상이 일어나 시스템이 다운될 수도 있어요.
현재 Makers에서는 메모리 941MB, 스왑 메모리 2GB를 활용하고 있어 메모리가 충분하지는 않은 환경입니다. 따라서 어느 종류의 데이터를 캐시에 저장할지, 얼만큼 데이터를 캐시에 저장할지, 얼마동안 오래된 데이터를 캐시에서 제거하는지에 대한 전략에 대해서도 알아보려고 해요.
## 캐싱의 문제점
캐싱을 사용하면 데이터 정합성이라는 문제가 발생할 수 있어요. 어느 한 데이터가 캐시와 데이터베이스에서 같은 데이터를 요청했음에도 데이터 정보 값이 다를 수도 있다는 의미에요. 따라서 적절한 캐시 읽기 전략과 캐시 쓰기 전략을 활용하여 캐시와 DB간의 데이터 불일치 문제를 극복하면서도 빠른 성능을 잃지 않도록 하려고 합니다.

## 캐시 전략 선택하기
현재 makers는 was 서버 한 대로 서비스하고 있어 로컬 캐시를 사용하여도 된다. 하지만 scale-out 가능성을 감안해 Redis를 이용한 글로벌 캐시를 사용해보도록 한다.

![[캐시 읽기 전략]]
## 캐시 저장 방식
캐시 솔루션은 자주 사용되면서 자주 변경되지 않는 데이터의 경우 캐시 서버에 적용하여 반영할 경우 높은 성능 향상을 이뤄낼 수 있다
또한 캐시 솔루션은 언제든지 데이터가 날라갈 수 있는 휘발성을 기본으로 한다. 이는 데이터를 주기적으로 디스크에 저장함으로서 어느정도 해결을 볼수는 있지만, 실시간으로 모든 데이터를 디스크에 저장할 경우 성능 저하를 일으킬 수 있어 어느 정도 데이터 수집과 저장 주기를 가지도록 설계해야 한다. 즉, 데이터의 유실 또는 정합성이 일정 부분 깨질 수 있다는 점을 항상 고려해야 한다.

따라서 친구 추천 api에서 외부 api를 통해 조건에 맞는 친구의 리스트를 가져오는데, 이는 자주 리프레시 되는 api라 자주 사용되지만, sopt 앱 유저를 보았을 때, 학교와 기수, MBTI는 변경이 매우 적고, 따라서 학교 별, 기수 별, MBTI 별 유저의 id 리스트는 자주 사용되면서 자주 변경되지 않아 캐싱에 적합할 것으로 판단되었습니다. 
또한 데이터가 휘발되더라도 다시 외부 api에서 값을 받아오면 되기 때문에 좋은 전략이라고 판단되었습니다.
## 캐시 제거 방식
아무리 학교, 기수, MBTI가 잘 변경이 되지 않는 값이라고는 하지만, 기수는 sopt의 기수가 변경될 때마다 변경 될 수 있는 값이며 MBTI도 유저가 프로필 변경에서 쉽게 변경할 수 있는 값입니다. 따라서 기본 만료 정책을 설정하여 기간이 만료되면 캐시에서 데이터가 제거 되고, 다시 값을 가져오도록 하고자 합니다. 
하지만, 캐시 만료 주기가 너무 짧으면 데이터는 너무 빨리 제거되어 캐시를 사용하는 이점이 점차 줄어들고, 반대로 너무 기간이 길면 데이터가 변경될 가능성과 메모리 부족 현상이 발생합니다.

따라서 적절한 시간을 고민해본 결과 하루 정도 단위면 어떨까라는 생각을 해보았어요 학교와 기수는 거의 변경되지 않고, 플레이그라운드 프로필을 변경해야만 변경되는 MBTI 값도 유저들이 거의 변경하지 않는 값이기 때문입니다. 
정합성이 최대 하루동안 맞지 않을 수도 있다는 단점이 있지만, MBTI별 추천을 하더라도 동일한 MBTI를 가진 유저가 랜덤으로 n명 추출되기 때문에 유저가 MBTI 변경된 유저가 하루 뒤에 적용된다는 사실은 인지하기는 어려울 것이라고 판단하였습니다.