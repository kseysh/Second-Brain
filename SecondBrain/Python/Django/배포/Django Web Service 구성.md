# Django project 구성
![[img.jpg|400]]

Web Service와 WAS에 대해서 => [[WS와 WAS의 차이]]

Reverse Proxy Server란? => 

## WSGI server란?
wsgi => web server gateway interface
=> 웹서버와 파이썬을 사용한 웹 어플리케이션 개발환경 간의 인터페이스에 대한 표준규칙 즉 파이썬의 웹 어플리케이션 서버 (WAS)

	**HTTP requests** -> **Web Server** -> **WSGI Server**(Middleware) -> **Django**(WSGI를 지원하는 Web Application)

이런 식으로 동작이 이루어지며 uwsgi/gunicorn을 WSGI Server라고 할 수 있다.

![[Pasted image 20230702220252.png]]



