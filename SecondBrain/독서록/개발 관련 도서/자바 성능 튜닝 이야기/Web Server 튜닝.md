- 웹 서버가 사용하는 스레드의 개수
	- 적절하게 사용할 수 있는 개수로 지정한다.
- 최대 요청 개수
	- 0이면 제한을 두지 않겠다는 말이다
	- 가급적 기본값인 0을 사용한다.
- 서버를 띄울 때 프로세스의 개수
	- 프로세스의 개수 x 프로세스당 스레드의 수를 이용해 요청 개수를 계산한다.
- 최대 처리 가능한 클라이언트의 수
	- 프로세스의 개수 x 프로세스당 스레드의 수로 세팅한다.
- 프로세스당 스레드의 수
- KeepAlive
	- 웹 서버와 웹 브라우져에서 연결을 재사용하기 위해 사용한다.
- KeepAliveTimeout
	- KeepAlive 설정시 반드시 설정해둔다.
