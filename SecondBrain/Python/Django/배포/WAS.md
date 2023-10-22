# WAS (Web Application Server)
정적인 리소스를 WS가 처리한다면 동적인 리소스는 어떻게 처리하느냐?
그것을 해결해줄 수 있는 것이 WAS이다. 

### Web Application이란?
웹에서 실행되는 응용 프로그램을 뜻한다.

### Web Application Server란?
jsp, php같은 언어들을 사용해 동적인 페이지를 생성할 수 있는 서버
프로그램 실행 환경과 데이터베이스 접속 기능을 제공하며 비즈니스 로적을 수행할 수 있어야 한다.

## Web Server + Web Container = WAS?
java에서는 was를 웹 어플리케이션 컨테이너라고 부르기도 한다. 그렇다면 웹 어플리케이션이란 뭘까?

### Web Container란?
jsp, servlet을 실행시킬 수 있는 소프트웨어이다.


#### WAS 동작 환경
![[Pasted image 20230702210921.png]]
Client가 요청을 보내게 되면 이게 정적으로 처리해야 하는지 동적으로 처리해야하는지를 Web Server가 판단한다. 
정적으로 처리해야한다면 결과 값을 바로 client로 전송해주고,
동적으로 처리해야한다면 값을 Web Container로 이동한다. Web Container에서는 동적데이터를 처리해 다시 Web Server로 이동시켜주고, 결과값을 client로 전송해준다.