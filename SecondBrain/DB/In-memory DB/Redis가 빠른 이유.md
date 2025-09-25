# 1. Redis의 특별한 자료구조
#### B Tree의 단점
- 복잡하다
- 리백런싱의 비용이 높다
- 멀티 태스킹 환경에서 사용하기 복잡하다
Skip List를 이용해 B Tree보다 동시성 문제를 더 잘 해결할 수 있도록 하였다.
## Skip List의 특징
- 키들이 정렬되어 있다.
- O(log n) 수준 복잡도를 가진다
- 각각의 상위 레벨은 하위 레벨보다 절반 개수의 원소를 가진다.
## Skip List 장점
- 모든 연산 (검색, 삭제, 삽입)에 대해 O(log N) 복잡도가 보장된다.
- B Tree 밸런싱 비용에 비해 구현하기 쉽고 비용이 적다.
- B Tree에 비교하여 병렬 처리가 더 용이하다.
### 단점
- 각각의 레벨에 대해서 많은 수의 링크를 필요로 하기 때문에 공간이 추가적으로 필요하다.
## Sorted set에서 우선순위 큐가 아닌 Skip List를 사용하는 이유는?
=> Linked List에 대한 검색 시간을 최적화하기 위함

[참고](https://velog.io/@redjen/%EB%A0%88%EB%94%94%EC%8A%A4%EB%8A%94-%EC%99%9C-%EB%B9%A0%EB%A5%BC%EA%B9%8C) 
[[Redis Sorted Set]]
## 2. epoll  IO 모델
레디스는 요청이 들어올 때마다 커넥션과 스레드를 생성하는 것이 아니라 epoll IO multiplexing을 이용해 처리한다.
[[epoll]]

1. 여러 명의 유저가 클라이언트 소켓을 통해 Redis 서버에 연결
	1. Redis는 이 fd들을 epoll에 등록
2. 유저들이 요청을 보내면 RB Tree에서 fd들을 찾아 Ready List에 보낸다.
3. Redis 이벤트 루프에서 epoll_wait 호출 -> fd들 반환
4. Redis는 반환된 fd 각각에 대해 등록된 handler 실행
5. 응답은 다시 epoll에 EPOLLOUT 이벤트로 등록되어, 소켓이 쓰기 가능할 때 응답 전송 (TCP의 send buffer가 가득찰 수 있으므로)

## 3. 인 메모리 데이터베이스
데이터 스토리지가 가지는 계층 구조 덕분에 더 빠른 읽기 / 쓰기 성능을 가진다.