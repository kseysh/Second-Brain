멋쟁이 사자처럼 해커톤에서 소셜로그인 api를 만들고 회원가입을 어떻게 처리할지 고민하다가 [[JWT]]를 써보기로 했습니다.  session_id를 이용한 로그인 방식은 저번 해커톤에서도 사용해보기도 하였으며, JWT쪽이 보안을 더 강화할 수 있다는 말에 JWT방식으로 회원가입 및 로그인 로직을 구현해보고 싶어졌습니다.

jwt는 django rest_framework의 simple jwt를 사용하기로 했습니다.
## simple jwt 사용 설정

### 모듈 설치
`pip install djangorestframework-simplejwt`를 통해 simplejwt 모듈을 설치해줍니다. 
( 블로그에서 `djangorestframework-jwt`를 사용하기도 하는데 이는 Release History가 2017년이 마지막이므로 비교적 최신인 `djangorestframework-simplejwt`를 사용하는 것을 권장합니다. )
### settings.py 설정

```
INSTALLED_APPS = [
    ...
    'rest_framework_simplejwt',

    'rest_framework_simplejwt.token_blacklist',
	...
]
```
여기서 `rest_framework_simplejwt.token_blacklist`는 access token을 refresh token로 새로 재 발급할 때 원래 있던 access token이 탈취당하여 사용되지 않도록 블랙리스트로 저장하여 사용하지 못하도록 하는 방법입니다. refresh 토큰 또한 유저가 로그아웃하거나 하면 유저 이외의 사람이 사용하지 못하도록 블랙리스트로 저장해놓고는 합니다.

```
REST_FRAMEWORK = { 
	... 
	
	'DEFAULT_PERMISSION_CLASSES': (
        # 'rest_framework.permissions.IsAuthenticated', # 인증된 사용자만 접근
        # 'rest_framework.permissions.IsAdminUser', # 관리자만 접근
        'rest_framework.permissions.AllowAny', # 누구나 접근
	),
	
	'DEFAULT_AUTHENTICATION_CLASSES': (  
		'rest_framework_simplejwt.authentication.JWTAuthentication', 
	) 
	... 
}
```
DEFAULT_PERMISSION_CLASSES를 AllowAny로 설정하여 API에 일일히 접근권한을 설정하지 않도록 합니다. 로그인이 필요한 부분에만 fbv라면 def 위에 `@permission_classes([permissions.IsAuthenticated])`를 설정해주고,
cbv라면 permission_classes = [IsAuthenticated] 이렇게 추가하여 접근권한을 설정해줍니다.
그리고는 기본 유저 인증 클래스를 JWTAuthentication으로 설정해놓습니다. 

아래는 추가적인 JWT 설정입니다.
```
SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(hours=5), # access token의 유효시간 
    'REFRESH_TOKEN_LIFETIME': timedelta(days=7), # refresh token의 유효시간
    'ROTATE_REFRESH_TOKENS': True, # True일시 새로운 리프레시 토큰이 발급될 때마다 이전의 리프레시 토큰이 만료된다.
    'BLACKLIST_AFTER_ROTATION': True, # 리프레시 토큰이 새로 발급되면 이전의 리프레시 토큰을 블랙리스트에 추가하는 옵션
    'UPDATE_LAST_LOGIN':True, # True일시 마지막 로그인 시간을 업데이트한다.
    'ALGORITHM': 'HS256', # JWT 암호화 알고리즘 지정
    'SIGNING_KEY': SECRET_KEY, # SECRET_KEY를 이용해 JWT 서명에 사용되는 비밀키를 지정한다.
  
    'USER_ID_FIELD': 'username', # 사용자 모델에서 사용자를 식별하는 필드 지정
    'USER_ID_CLAIM': 'username', # 사용자 모델에서 사용자를 식별하는 클레임 이름 지정
  
    'TOKEN_USER_CLASS': 'accounts.User', # JWT 토큰에 저장되는 사용자 정보의 클래스를 지정
}
```
## 커스텀 유저 설정

```
from django.db import models
from django.utils.translation import gettext_lazy as _
from django.contrib.auth.models import AbstractUser, BaseUserManager

class UserManager(BaseUserManager):
  
    def create_user(self, username, **extra_fields):
        if not username:
            raise ValueError(_('The username must be set'))
        user = self.model(username=username, **extra_fields)
        user.set_password("temp_password")
        user.save()
        return user
  
    def create_superuser(self, username, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)
        extra_fields.setdefault('is_active', True)
  
        if extra_fields.get('is_staff') is not True:
            raise ValueError(_('Superuser must have is_staff=True.'))
        if extra_fields.get('is_superuser') is not True:
            raise ValueError(_('Superuser must have is_superuser=True.'))
        return self.create_user(username, **extra_fields)

class User(AbstractUser):
    username = models.CharField(verbose_name='유저 아이디',max_length=64,unique=True)
    user_email = models.EmailField(verbose_name= "유저 이메일",max_length=255,blank=True,null=True)
    user_property = models.IntegerField(verbose_name= "유저 자산",default=3000000)
    user_nickname = models.CharField(verbose_name="유저 닉네임",max_length=32)
    USERNAME_FIELD = 'username'
    REQUIRED_FIELDS = []
  
    objects = UserManager()

    def __str__(self):
        return self.username
```
BaseUserManager와 AbstractUser를 이용해서 커스텀유저를 만들었습니다.

여기서 UserManager는 User를 생성하고 관리하는데 필요한 클래스로 이렇게 User와 UserManager를 분리하여 커스텀유저를 만드는 것이 사용자 관리에 있어 Django의 권장 사항으로 이렇게 분리해서 만드는 것을 추천합니다.

## 쿠키를 이용한 JWT 통신 방식

### 회원가입 및 로그인
아래는 저번에 포스팅한 kakao callback 함수의 일부입니다.
try 부분은 회원가입이 완료되어 있는 user에 로그인하는 함수이고,
except 부분은 회원가입이 되지 않아 User.DoesNotExist 에러가 나게 되면 회원가입 후 access token을 발급해주는 코드입니다.
set_cookie를 통해 클라이언트 쪽에 쿠키를 넣어주고 logout시에는 delete_cookie를 이용해 클라이언트의 쿠키를 다시 회수합니다. 이와 관련해서는 [[프론트와 백엔드의 쿠키 공유]]를 보면 쉽게 이해하실 수 있습니다.

```
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

### 로그아웃
delete_cookie를 이용해 cookie를 삭제해줍니다. 이는 프론트엔드에서 withcredential 설정이 있어줘야 서버에서 쿠키를 가져갈 수 있는 권한이 생깁니다.
```
@api_view(['GET'])
def kakao_logout(self):
    response = Response({
        "message": "Logout success"
        }, status=status.HTTP_202_ACCEPTED)
    response.delete_cookie("accessToken")
    response.delete_cookie("refreshToken")
  
    return response
```


### 유저 정보 확인
check_jwt에서는 request에서의 COOKIES의 accessToken을 받아 그 jwt 토큰을 디코딩하고 username을 받아 user를 찾고 serializer를 통해 반환해줍니다.
만약 토큰이 만료되었다면 request의 refresh 토큰을 이용해 새로운 access 토큰을 재발급해줍니다.
```
def user_info(request):
    try:
        access = request.COOKIES['access']
        payload = jwt.decode(access, settings.SECRET_KEY, algorithms=['HS256'])
        username = payload.get('username')
        user = get_object_or_404(User, username=username)
        serializer = UserSerializer(instance=user)
        return Response(serializer.data, status=status.HTTP_200_OK)

    except(jwt.exceptions.ExpiredSignatureError):
        data = {'refresh': request.COOKIES.get('refresh', None)}
        serializer = TokenRefreshSerializer(data=data)
        if serializer.is_valid(raise_exception=True):
            access = serializer.data.get('access', None)
            refresh = serializer.data.get('refresh', None)
            payload = jwt.decode(access, settings.SECRET_KEY, algorithms=['HS256'])
            username = payload.get('username')
            user = get_object_or_404(User, username=username)
            serializer = UserSerializer(instance=user)
            res = Response(serializer.data, status=status.HTTP_200_OK)
            res.set_cookie('access', access)
            res.set_cookie('refresh', refresh)
            return res
```

