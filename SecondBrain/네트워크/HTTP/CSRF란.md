> CSRF(Cross-Site Request Forgery)는 웹 애플리케이션 취약점 중 하나로 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 해서 특정 웹페이지를 보안에 취약하게 한다거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법

그렇다면, Cross-Site란 무엇일까?

## Cross-Site
![[Pasted image 20230814120104.png]]

Site는 scheme, 최상위 도메인(TLD)와 최상위 도메인 바로 앞의 도메인 (TLD+1)의 조합이며, 이 중 하나라도 다르다면 Cross Site라고 한다. 

위의 예시에서는 https://example.com 이 Site이다.

## csrf를 막기 위한 방어 매커니즘

### CSRF Token
CSRF Token은 보통 중요한 기능에 대해 CSRF를 대응하기 위해 많이 사용되는 방법이며, 기능을 수행하기 위한 페이지에서 추측할 수 없는 길이의 임의 토큰을 발급하고, 해당 토큰을 포함한 요청만 처리하도록 하여 해당 기능이 실행되어야 하는 곳에서만 처리될 수 있도록 제한하는 기술이다.

###  Token + Cookie
CSRF Token의 보안성을 강화하기 위한 방법으로 CSRF Token을 header 또는 body 값을 통해 전달하는 방법 이외에 HttpOnly Cookie로도 전달하여 이중으로 검증하는 방법이 있습니다. 이렇게 되는 경우 어떤 액션을 통해 CSRF Token이 유추되거나 탈취되어도 보안성이 요구된 Cookie를 통해 동시에 전달할 수 없기 때문에 2차적인 피해를 예방할 수 있습니다.

### Captcha
Captcha(Completely Automated Public Turing test to tell Computers and Humans Apart)는 보통 봇을 검증하기 위해 사용하는 장치입니다. 일반적으로 사람이 식별할 수 있는 형태의 이미지를 통해 글씨를 유추하거나 컴퓨터가 판단하기 어려운 문제를 통해 사람임을 증명하는 기술인데, 이를 통해서도 CSRF에 대응할 수 있습니다.

### Cookie protection - SameSite
SameSite를 Lax 또는 Strict로 지정해주면 Cross-origin 환경에서 쿠키 전송이 불가능해집니다. 이를 이용하면 쉽게 CSRF를 막을 수 있습니다.

참고한 사이트
https://www.hahwul.com/cullinan/csrf/
