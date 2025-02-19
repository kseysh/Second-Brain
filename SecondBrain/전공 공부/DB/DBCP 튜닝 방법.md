# DBCP가 필요한 이유
커넥션의 연결 생성, 해제는 TCP의 3-way-handshake, 4-way-handshake 탓에 느리다.
따라서 DBCP를 이용해 DB와 커넥션을 미리 맺어놓고 필요할 때 풀에서 가져오고 끝나면 반환하는 형식으로 진행한다.
## 중요 튜닝 point
![[Pasted image 20250219191220.png|400]]
### DBCP 설정
#### minimumIdle

#### maximumPoolSize

#### maxLifetime

#### connectionTimeout

### DB 서버 설정
#### max_connections

#### wait_timeout
