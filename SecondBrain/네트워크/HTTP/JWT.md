멋쟁이사자처럼 중앙해커톤에서 JWT를 로그인 과정에서 사용하며 이해한 내용을 정리하려고 합니다.


# JWT란?
JWT는 Json Web Token의 줄임말으로 서버가 시크릿 키와 데이터를 이용해 고유한 문자열을 생성하고 이를 통해 클라이언트의 권한을 검증하게 된다.

## JWT의 구성 요소
jwt는 `(header).(payload).(signature)` 의 구성을 지닌다.

### Message (Header, Payload)
Header와 Payload부분을 합쳐 Message라고도 부르기도 한다 Message에는 서버가 보내야 하는, 보내고 싶은 정보가 담겨있으며 `Base64`로 암호화 되어있어, 누구나 획득하기만 한다면, 복호화하여 정보를 볼 수 있다.

> 따라서 복호화를 통해 정보를 볼 수 있으므로 JWT에는 중요한 정보를 넣으면 안된다!

### Header
Message의 일부인 header부분을 살펴보도록 하겠습니다.
Header에 들어가는 내용은
- `<alg>` : 암호화를 어떻게 하였는가, 이 토큰은 무슨 토큰인가 등의 내용을 명시합니다. 암호화에는 RS256, HS256등이 있습니다.
>RS256 : sha256 + 비대칭키를 사용하는 암호화 방식
  HS256 : sha256 + 대칭키를 사용하는 암호화 방식
  대칭키 : 키 하나로 복호화와 암호화를 하는 것
  비대칭키 : 복호화와 암호화를 서로 다른 키로 하는 것
- `<typ>`: Token의 Type을 명시합니다. JWT토큰을 사용하므로 JWT라고 명시되어 있습니다.
### Payload
`Payload`는 `Header`와 다르게 종류가 3개 있다.

- `Registered claims`
    - `<iss>` : 토큰 발행자 (Issuer)
    - `<sub>` : 토큰 제목. (Subject)
    - `<aud>` : 토큰 대상자. (Audience)
    - `<exp>` : 토큰 만료시간. Unix Time으로 적는다. (Expiration Time)
    - `<nbf>` : 토큰이 활성되는 시간. 이 시간 이후에 토큰을 사용할 수 있다.  
        이도 Unix time이다.
    - `<jti>` : JWT의 고유 식별자이다. 다른 JWT와 다른 Unique한 값을 지녀야 한다.
- `Public claims`
    - JWT를 사용하는 사람들에 의해 정의되는 Claim이다.
    - [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml)에 정의된 Claim Name을 사용하거나 다른 사람들과의 충돌을 방지하기 위해, URI를 사용해야한다.
- `Private Claims`
    - [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml)에 정의되어 있지 않는 Claim Name이면 뭐든지 사용 가능하다. 
    - JWT 사용자와 생산자 상호 합의 하에 정해지는 claim이다.
### Signature
Signature는 데이터의 무결성을 입증하는데 사용한다.
[[데이터 무결성]]
데이터가 중간에 탈취되어, Payload나 Header가 변경되어도,  
Signature 검증을 통해 변경되었다는 것을 알 수 있다.
## JWT 그거 왜 사용하는 건데?

### Stateless
클라이언트와 서버의 연결상태를 state라 하며 Stateless는 클라이언트와 서버가 연결되던 되지 않던 신경쓰지 않는 상태를 말한다.

>클라이언트와 서버의 연결 상태를 내가 관여한다 => Stateful  
  클라이언트와 서버가 연결되던 아니던 신경쓰지 않는다 => Stateless

Stateless를 사용하는 이유는 확장성과, 자원에 있다.
Stateless는 서버 확장시에 클라이언트의 연결 정보를 옮길 필요가 없어 확장이 편하고, 서버가 연결 유지를 담당하지 않기 때문에 자원이 Stateful에 비해 적게 들어간다는 장점이 있다.



참고한 사이트
https://gpalektzja.medium.com/jwt%EB%A5%BC-%EC%B0%8D%EB%A8%B9%ED%95%B4%EB%B3%B4%EC%9E%90-2bf26656aa42