# start-line 시작라인
### Start-Line

**예시)** PATCH /member HTTP/1.1

1. Method
    
    `HTTP Method`가 들어간다.
    
1. Path
    
    Host(Base Url) 뒤에 어떠한 경로로 요청을 보내는지에 대한 정보가 나와있다.
    
3. Version Of Protocol
    
    프로토콜의 버전이 명시되어 있습니다.
# header (헤더)
### Header

HTTP Header에는 요청/응답에 대한 각종 정보들이 포함된다.

대표적으로 `Content-Type` 이 있다. `Content-Type` 은 해당 자원이 어떤 media type을 갖는지 명시해주는 역할을 한다.

![[Untitled.png]]

또한 회원가입 및 로그인을 구현하게되면, 요청 시에 유저임을 확인하기 위한 정보로 `Authorization` 이라는 Header안에 정보를 넣을 수 있다.
# empty line (CRLF)
# message body