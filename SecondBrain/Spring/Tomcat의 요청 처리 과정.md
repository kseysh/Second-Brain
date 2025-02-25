## 1. Connector의 요청 수신
Connector가 요청을 리스닝 하고 있다.
BIO, NIO, APR 방식 중 하나를 사용해 요청을 받는다 (보통 NIO를 이용해 멀티플렉싱 사용)
## 2. Acceptor의 요청 수락
요청이 들어오면 Acceptor 스레드가 요청을 요청 큐에 넣는다.
Executor에서 요청을 처리할 수 있도록 대기 상태로 둔다.
## 3. Worker의 요청 처리
요청 큐에 있는 요청을 Worker가 하나씩 꺼내 처리한다.
Worker 스레드는 스레드 풀 기반으로 동작
## 서블릿에서 처리한 응답 반환