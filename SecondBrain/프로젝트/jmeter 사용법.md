## 1. Thread Group 추가
- 만든 테스트에 오른쪽 클릭 -> Add -> Threads (Users) -> Thread Group

- Number of Threads
	- 테스트에 사용될 스레드의 개수
- Ramp-up period
	- 스레드 개수를 만드는데 소요되는 시간
- Loop count
	- Number of Threads X Ramp-up period 만큼 요청을 다시 보낸다
## Sampler 추가
- 사용자의 액션
- Thread Group 우클릭 -> Add -> Sampler -> HTTP Request 클릭
## Listener 추가
- 응답을 받아 reporting, 검증, 그래프 등 다양한 처리 지원
- HTTP Request에 오른쪽 클릭 -> Add -> Listener -> View Results Tree, Summary Report, View Results in Table 생성
## Configuration
Sampler 또는 Listener가 사용할 설정 값
## Assertion 
- 응답 확인 방법
- HTTP Request 우클릭 -> Add -> Assertions -> Response Assertion 클릭



## 유의 사항
JMeter를 돌리는 서버와 WAS가 같으면 같은 메모리를 사용하여 정확한 값을 측정할 수 없을 수 있다.
