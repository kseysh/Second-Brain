친구 추천에는 3가지 유형이 있음
GENERATION
MBTI
UNIVERSITY

![[Pasted image 20250305170716.png]]
여기서 for(FriendRecommendType type : typeList) 부분을 비동기 CompletableFuture를 이용해서 개선해보자!

사실 현재 redis를 이용해 이미 캐싱되어 있는 값이라서 큰 변화는 없을 수도 있지만...