![](https://blog.kakaocdn.net/dn/dnydPv/btrNWqJpTYf/ncp6n5aECBKDqB2TuvKr2K/img.png)

# Jmeter의 구성요소
### Thread Group
스레드를 관리하는 곳
스레드 수, 즉 가상의 사용자 수를 설정할 수 있다.
### Samplers
요청을 추상화한 것
FTP, HTTP, SMPT등 다양한 요청 유형을 선택할 수 있다.
### Logic Controller
요청들을 묶어 시나리오화한 것
하나의 Logic Controller에 여러 HTTP 요청 Samplers들을 순차적으로 담음으로써  사용자의 실제 사용 예상 시나리오와 유사하게 테스트 환경을 구성할 수 있다.
### Listeners
테스트 결과를 추상화한 것
그래프, 테이블 등 여러 형태로 테스트 결과를 시각화해준다.
### Configuration Elements
테스트 환경에 적용될 설정
HTTP 요청 Sampler를 이용한 테스트를 할 때, 헤더에 토큰을 담거나, 쿠키에 세션을 담음으로써  로그인된 사용자의 요청으로 만들기 위한 쿠키, 헤더 값을 설정할 수 있다.
