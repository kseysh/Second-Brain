## TokenAuthentication
TokenAuthentication을 사용하기 위해서 INSTALLED_APPS에 `rest_framework.authtoken`를 넣어주어야 한다.

```
INSTALLED_APPS = [
    ...
    'rest_framework.authtoken'
]
```

## 토큰 만들기

```
from rest_framework.authtoken.models import Token

token = Token.objects.create(user=...)
print(token.key)
```

Client에게 권한을 부여하기 위해서 토큰 key는 HTTP 헤더 안에 Authorization을 지니고 있어야 한다. 
key는 두 문자열을 구분하는 공백으로 문자열 Token을 접두어로 지정해야 한다.
ex)
```
Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b
```

header에서 `Bearer`같은 다른 키워드를 사용하기를 원한다면, keyword 변수를 세팅하면 된다.













