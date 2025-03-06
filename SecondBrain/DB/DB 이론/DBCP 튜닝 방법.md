# DBCP가 필요한 이유
커넥션의 연결 생성, 해제는 TCP의 3-way-handshake, 4-way-handshake 탓에 느리다.
따라서 DBCP를 이용해 DB와 커넥션을 미리 맺어놓고 필요할 때 풀에서 가져오고 끝나면 반환하는 형식으로 진행한다.
## 중요 튜닝 point
![[Pasted image 20250219191220.png|400]]
DBCP에서는 **minimumIdle과 maximumPoolSize를 동일하게 하여 pool size를 고정하는 것을 권장**한다.
=> 사용자 수가 급상승하면 Connection pool의 대기 시간이 오래 걸릴 수 있기 때문
DBCP의 **maxLifetime은 DB의 connction time limit보다 몇 초 짧게 설정**해야 한다. (요청이 가는 동안 maxLifetime을 초과하면 연결이 끊기기 때문)
# 적절한 connection 수 찾기
- 모니터링 환경 구축 (서버 리소스, 서버 스레드 수, DBCP 등등)
- 백엔드 시스템 부하 테스트
- request per second와 avg response time 확인
- ![[Pasted image 20250219193006.png|300]]
	- 표시한 부분에서 모니터링 지표들이 어떻게 되는지 확인해야 한다!
	- 백엔드 서버, DB 서버의 CPU, MEM 등등 리소스 사용률 확인
		- 백엔드 서버의 부하가 크면 서버 추가를 고려한다.
		- DB 서버의 부하가 크면 샤딩, 파티셔닝, 캐시 레이어 도입 등을 고려한다.
	- WAS와 DB가 다 좋고 thread per request 모델이라면 active thread 수를 확인한다
		- maximum thread pool과 active thread수가 같다면 스레드 풀의 개수가 너무 작아서의 문제일 수도 있다.
- DBCP의 active connection 수를 확인한다
	- maximumPoolSize와 active connection수가 같다면 maximumPoolSize를 늘려보면서 해야한다.
- 사용할 백엔드 서버 수를 고려하여 DBCP의 max pool size 결정
