> CORS(Cross-Origin Resource Sharing)는 웹 보안 정책 중 하나로, 다른 출처(origin)에서 리소스에 접근하는 것을 제어하는 메커니즘

그렇다면, Cross Origin이란 무엇일까?

## Cross-Origin

### Origin

![[Pasted image 20230814114723.png]]
Origin은 Scheme, Host name, Port의 조합이며, 이중 하나라도 다르다면, 이를 Cross Origin이라고 한다.

브라우저는 Same-Origin-Policy(동일 출처 정책)를 통해 다른 Origin과의 리소스 공유를 제한하여 보안을 위협하는 악의적인 파일이나 사이트로부터 보호할 수 있다. 쿠키는 특히 증명과 관련된 중요한 정보이기 때문에, 함부로 주고받을 수 없도록 제한된다.

따라서 백엔드에서 프론트엔드로 Set-Cookie를 통해 쿠키를 전달해주더라도 믿을 수 없는 Origin으로부터 온 것이라면 저장하지 않고, 저장되어 있는 쿠키가 있다고 해도 요청할 때 포함하여 보내지 않게 한다.

이를 해결하기 위해서 클라이언트와 서버 모두 증명과 관련된 옵션을 통해 해당 정보를 주고 받겠다고 설정해주어야 한다.

클라이언트에서는 증명 정보(쿠키)를 포함한 요청을 보내기 위해, `withCredentials` 옵션을 true로 설정해야 한다.

서버에서는 증명 정보를 포함한 요청을 허용하기 위해 `Access-Control-Allow-Credentials`를 true로 설정해준다.

django에서는 `Access-Control-Allow-Credentials`를 true로 설정해주기 위해 아래와 같이 설정을 해준다.

```
CORS_ALLOW_CREDENTIALS = True
```
위의 설정을 해준다면 브라우저에서 CORS 요청 시 자격 증명 정보 (ex) 쿠키, 인증 헤더)를 포함할 수 있도록 해준다. 즉, 인증 정보가 필요한 요청에 대해 자격 증명이 가능한 상태로 요청을 보낼 수 있다.

또한 CORS 요청을 보낼 수 있는 ORIGIN들도 django에서 설정을 해주어야 CORS를 허락 받을 수 있다.
```
CORS_ALLOWED_ORIGINS = [
    'http://localhost:3000',
	'http://localhost:8000',
    'http://127.0.0.1:8000',
    'https://stalksound.store',
    'https://stalk-login-test.pages.dev',
]
```
이 설정을 해주게 되면 위에 적혀있는 특정 ORIGIN들만 CORS 요청을 허용할 출처 목록으로 지정하게 된다. 포함된 출처만이 해당 웹 애플리케이션의 리소스에 CORS 요청을 보낼 수 있게 된다.

`CORS_ORIGIN_WHITELIST`도 `CORS_ALLOWED_ORIGINS`와 비슷한 기능을 제공하지만 튜플의 리스트로 출처와 연결된 추가적인 설정을 지정할 수 있다고 한다. 이를 통해 더 세밀한 제어를 할 수 있지만 추가적인 설정은 필요하지 않아 `CORS_ALLOWED_ORIGINS`로만 설정을 해주었다.

`CORS_ORIGIN_ALLOW_ALL = True` 설정으로도 CORS 요청을 허용해줄 수 있지만 이 설정은 모든 origin에서의 요청이 허용되는 설정으로 보안상 권장되는 설정은 아니다. 




