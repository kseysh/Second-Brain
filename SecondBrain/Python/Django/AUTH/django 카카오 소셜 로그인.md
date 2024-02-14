멋쟁이 사자처럼 해커톤을 준비하며 시각장애인을 위한 주식 차트 음성화 서비스를 기획하며 시각장애인 분들은 회원가입 및 로그인 조차 어려울 것 같아 최대한 간단한 로그인과 회원가입을 구현하기 위해 소셜로그인을 통한 로그인으로 서비스를 기획하고자 하여 카카오 소셜 로그인 api를 개발하게 되었습니다. 많이 어려웠고 고생했던 만큼 열심히 글을 작성하며 시행착오를 공유할테니 잘 봐주시길 바랍니다..!

# 카카오 개발자 애플리케이션 등록
카카오 개발자 애플리케이션(https://developers.kakao.com/console/app)에서 애플리케이션을 추가한다.

![[Pasted image 20230814172236.png]]

내 애플리케이션 > 앱 설정 > 요약 정보에서 REST API KEY를 잘 기억해둔다.

![[Pasted image 20230814172600.png]]


내 애플리케이션 > 제품 설정 > 카카오 로그인에서 활성화 설정의 상태를 ON으로 변경해준다.
아래의 OpenID Connect도 활성화를 해준다면 설명에 써있는 듯이 카카오 로그인 시 사용자의 인증 정보가 담긴 ID 토큰을 액세스 토큰과 함께 발급해준다.

![[Pasted image 20230814172658.png]]

아래에는 Redirect URI가 있는데 이는 조금 있다가 언급하기로 하자

![[Pasted image 20230814173227.png]]

다음으로는 내 애플리케이션 > 제품 설정 > 카카오 로그인 > 동의항목에서 카카오 로그인으로 서비스를 시작할 때 동의를 받아야 하는 항목들을 설정해준다.

![[Pasted image 20230814173955.png]]

# 카카오 로그인 시퀀스 다이어그램

카카오 로그인 시퀀스 다이어그램을 따라가면서 카카오 소셜로그인을 이해해봅시다다

![[Pasted image 20230814191426.png]]

django를 이용해 소셜로그인을 구현하여 django를 기준으로 설명하겠습니다.

## 카카오 로그인

사용자가 카카오 로그인 버튼을 누르게 되면( **카카오 로그인 요청** ) 아래 코드로 redirect를 보내서 카카오에게 인가 코드를 받아옵니다( **인가 코드 받기 요청** ).
`https://kauth.kakao.com/oauth/authorize?client_id={rest_api_key}&redirect_uri={KAKAO_CALLBACK_URI}&response_type=code` 
rest_api_key에서 내 애플리케이션 > 앱 설정 > 요약 정보에 있던 rest api key를 적어줍니다. 그리고 redirect uri로 인가코드를 받아 토큰 발급을 요청할 페이지를 설정해줍니다.

그러면 인가코드를 받기 위해 아래와 같은 창으로 이동하게 됩니다. ( 카카오 인증 서버의 **인증 및 동의 요청** ->클라이언트의 **로그인 및 동의** )

![[Pasted image 20230814192959.png]] 

클라이언트는 카카오 로그인을 통해 인가코드를 발급받고 redirect uri를 통해 쿼리스트링으로 key가 code인 인가코드를 가지고 redirect uri로 돌아옵니다( **인가 코드 발급** ). 

이후 redirect uri에서는 인가코드를 이용해 access 토큰 발급을 요청해줍니다. (OpenID Connect를 활성화 하였다면 ID token이 access 토큰에 담깁니다. )

access 토큰 발급을 위해 url이 `https://kauth.kakao.com/oauth/token`에 token header를 `'Content-type': 'application/x-www-form-urlencoded;charset=utf-8'` 로 설정해 준 후 data를 아래와 같이 설정해주어 access token을 받아줍니다.
```
'grant_type': 'authorization_code',
'client_id': rest_api_key,
'redirect_uri': KAKAO_CALLBACK_URI,
'code': code,
```
`grant_type`은 `authorization_code` 로 고정하고
`client_id`에는  rest api key를 넣어줍니다
`redirect_uri`에는 redirect uri를 넣어주고
마지막으로 `code`에는 받아온 인가코드를 넣어서 `POST`방식으로 보내주면 access token을 응답으로 반환해줍니다. ( 서비스 서버(django)에서 **인가 코드로 토큰 발급 요청** 및 카카오 인증 서버에서 **토큰 발급** )
받은 access 토큰은 `https://kapi.kakao.com/v2/user/me`의 url에 header에 `headers={"Authorization": f"Bearer ${access_token}",}`로 access token을 넣어주어 `GET`요청을 보내 유저 정보인 json 응답을 받습니다.  ( 서비스 서버의 **카카오 로그인 완료, 토큰 정보 조회 및 검증** 및 카카오 API서버의**요청 검증 및 처리** , 서비스 서버의  **제공받은 사용자 정보로 서비스 회원 여부 확인** ) 

이후 카카오에서 발급받은 id를 유저를 식별하는 id로 사용하여 JWT를 이용한 회원가입 및 로그인을 처리해주었습니다.

아래는 장고의 소셜로그인 코드입니다.

```
KAKAO_CALLBACK_URI = 'https://stalk-login-test.pages.dev/kakao/callback' # 프론트 배포 페이지의 callback uri

def kakao_login(request): # 백엔드 테스트용 login 코드 이 코드는 프론트에서 처리하도록 하기
    rest_api_key = settings.KAKAO_REST_API_KEY
    return redirect(
        f"https://kauth.kakao.com/oauth/authorize?client_id={rest_api_key}&redirect_uri={KAKAO_CALLBACK_URI}&response_type=code"
    )
```

위 코드는 로컬에서 소셜로그인을 진행할 때 카카오 로그인 요청 및 인가 코드 받기 요청을 하는 함수입니다. 프론트와 협업하며 소셜로그인을 진행하려면 이 부분은 프론트에게 맡겨 redirect uri를 프론트 측의 주소로 바꿔준 후 프론트엔드에서 kakao에게 인증 및 동의 요청을 보내고 로그인 및 인가 코드를 발급 받아, 인가 코드로 토큰 발급을 요청하여 받은 인가 코드를 백엔드(서버)에게 전달하여 백엔드가 토큰을 발급받고, 사용자를 인식하여 회원가입 및 로그인을 처리해주는 것이 일반적입니다. 
저도 이 부분은 로컬에서 테스트를 해볼 때에만 사용하였고 프론트엔드와 연결하여 redirect uri를 프론트엔드 쪽 주소로 넣어주고 활용하도록 하였습니다. 
> redirect uri 주의할 점
> redirect uri는 서버에서 설정한 redirect uri와 kakao developers에서 설정한 redirect uri가 같아야 합니다. 따라서 django에서 설정한 redirect uri가 내 애플리케이션 > 제품 설정 > 카카오 로그인에서 설정한 redirect uri안에 적혀있어야 합니다. 카카오 측에서 보안을 위해 다른 주소가 함부로 redirect되지 않도록 설정해 놓은 것이므로 필수적으로 확인해야합니다. 

django에서 설정한 redirect uri와 kakao developers의 redirect uri가 다를 경우 아래와 같은 에러가 발생합니다.
![[a2226b951ca989affeab32c7eca7b1492e999631.png|300]]


아래는 소셜로그인에서 가장 중요한 callback 함수입니다.
```
def kakao_callback(request):
    rest_api_key = settings.KAKAO_REST_API_KEY
    code = request.GET.get("code")
    kakao_token_uri = "https://kauth.kakao.com/oauth/token"
  
    request_data = {
            'grant_type': 'authorization_code',
            'client_id': rest_api_key,
            'redirect_uri': KAKAO_CALLBACK_URI,
            'code': code,
        }

    token_headers = {
            'Content-type': 'application/x-www-form-urlencoded;charset=utf-8'
        }
    token_req = requests.post(kakao_token_uri, data=request_data, headers=token_headers)
    token_req_json = token_req.json()
    error = token_req_json.get("error")

    if error is not None:
        raise ValueError(error)
    access_token = token_req_json["access_token"]
    profile_request = requests.get(
        "https://kapi.kakao.com/v2/user/me",
        headers={"Authorization": f"Bearer ${access_token}",})

    if profile_request.status_code == 200:
        profile_json = profile_request.json()
        error = profile_json.get("error")
        if error is not None:
            raise ValueError(error)
        username = profile_json["id"]
        user_nickname = profile_json['kakao_account']["profile"]["nickname"]
        user_email = profile_json["kakao_account"].get("email")
    else:
        raise ValueError(profile_request.status_code)
    try:
        user = User.objects.get(username=username)
        user_serializer = UserSerializer(user)
        token = TokenObtainPairSerializer.get_token(user)
        refresh_token = str(token)
        access_token = str(token.access_token)
        res = Response(
            {
                "user": user_serializer.data,
                "message": "login successs",
                "token": {
                    "access": access_token,
                    "refresh": refresh_token,
                },
            },
            status=status.HTTP_200_OK,
        )
        res.set_cookie("accessToken", value=access_token, max_age=None, expires=None, secure=True, samesite="None", httponly=True)

        res.set_cookie("refreshToken", value=refresh_token, max_age=None, expires=None, secure=True, samesite="None",httponly=True)
        return res
    except User.DoesNotExist:
        user = User.objects.create_user(username=username)
        user.username = username
        user.user_email = user_email
        user.user_nickname = user_nickname
        user.save()
        user_serializer = UserSerializer(user)
        token = TokenObtainPairSerializer.get_token(user)
        refresh_token = str(token)
        access_token = str(token.access_token)
        res = Response(
            {
                "user": user_serializer.data,
                "message": "register successs",
                "token": {
                    "access": access_token,
                    "refresh": refresh_token,
                },
            },
            status=status.HTTP_200_OK,
        )
        res.set_cookie("accessToken", value=access_token, max_age=None, expires=None, secure=True, samesite="None", httponly=True)

        res.set_cookie("refreshToken", value=refresh_token, max_age=None, expires=None, secure=True, samesite="None",httponly=True)

        return res
```

kakao의 access 토큰을 받기 위해서는 인가코드가 필요합니다. 프론트엔드에서 인가 코드를 발급받게 되면 프론트엔드가 인가 코드를 서버의  kakao_callback 함수에 전달해줍니다. 서버는 인가코드를 받아 `https://kauth.kakao.com/oauth/token`인 url에 아래와 같은 데이터와 헤더를 담아줍니다.
```
request_data = {
		'grant_type': 'authorization_code',
		'client_id': rest_api_key,
		'redirect_uri': KAKAO_CALLBACK_URI,
		'code': code,
	}
token_headers = {
		'Content-type': 'application/x-www-form-urlencoded;charset=utf-8'
	}
token_req = requests.post(kakao_token_uri, data=request_data, headers=token_headers)
```
여기서 `request_data`의 `grant_type`과 `token_headers`의 `Content-type`은 카카오에서 정해둔 고정값으로, 변경해서 사용하시면 안됩니다.
아래 kakao developers에 보시면 토큰을 받기 위해 어떤 값을 넣어야 하는지에 대해서 나와있습니다 어떤 메서드로 보내야하는지도 한 번 확인하신다면 좋을 것 같아요요
https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#request-token

제 코드를 따라하는 것도 좋지만 업데이트가 되어있을 수도 있으니 kakao developers에서 어떤 값을 보내야 하는지 확인을 해보고 그에 맞는 값을 넣어주시는 것을 추천드립니다! 저도 다른 블로거분이 하신 코드를 따라하다가 kakao developers에서는 post로 보내야 하는데 블로그에는 get으로 되어있는 등의 자잘한 에러가 난 적이 많아요 어떻게 동작하는지 이해하고 코드를 작성하는 것이 역시 느려보여도 제일 빠르게 가는 길인 것 같습니다.
![[Pasted image 20230815144625.png]]
![[Pasted image 20230815144641.png]]
![[Pasted image 20230815144653.png]]

아래와 같이 응답이 어떻게 오는지도 확인을 해볼 수 있습니다.
![[Pasted image 20230815144734.png]]

이렇게 토큰을 받고 그 토큰에서 access token을 추출하여 access token에서 사용자 정보를 추출하는 것도 비슷한 방식으로 하실 수 있습니다. 

이 방식은 한 번 kakao developers에서 확인하고 직접 해보시는 것을 추천드려요!
블로그를 작성하는 시기에는 header에 Authorization으로 Bearer 토큰으로 access_token을 보내는 방식입니다.
https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#req-user-info

이후에는 JWT를 이용하여 회원가입 및 로그인을 처리하는 로직인데요 이는 다음 포스팅에서 다루도록 하겠습니다. 
[[JWT를 이용한 회원가입 및 로그인]]


제 블로그를 보고 카카오 소셜로그인을 따라해보시는 것도 좋지만 가장 좋은 방법은 kakao developers의 문서를 보며 따라가시는 것이 가장 좋다고 생각합니다. 제 블로그는 이 글로만 남아있겠지만, kakao developers의 문서는 카카오 개발자분들이 열심히 업데이트 해주시고 어떻게 하면 잘 사용하실 수 있을까 많은 고민을 하고 적어두신 문서일거니까요. 제 코드는 참고만 해주시고 아 이렇게 사용하면 되구나~ 라는 점만 이해해주시고 kakao developers에 들어가셔서 문서를 보면서 아 `https://kauth.kakao.com/oauth/token`의 header에는 어떤 값이 고정 값으로 들어가야 하는구나 이런 글을 다 카카오 개발자 분들이 작성해주셨으니 글을 보며 소셜로그인을 시도해보신다면 뒤돌아보셨을 때 크게 성장하실 것이라고 생각해요 저도 이번에 소셜로그인을 진행하면서 공식 문서 보는 방법을 되게 많이 배웠고, 또 성장했다고 생각하거든요. 다음 글은 카카오 소셜로그인을 진행했으니, JWT를 통해 회원가입 및 로그인을 하는 방법을 포스팅 하도록 하겠습니다. 감사합니다!