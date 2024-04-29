> 라우터를 사용하는 이유 : 
> 주어진 리소스 컨트롤러에 대한 모든 공통 경로를 빠르게 선언하기 위함. 

## 사용 방식 (Simple Router)

```
from rest_framework import routers

router = routers.SimpleRouter()
router.register(r'users', UserViewSet)
router.register(r'accounts', AccountViewSet)


urlpatterns = [
    path('forgot-password', ForgotPasswordFormView.as_view()),
    
    path('', include(router.urls)),
    # 방식 1
	path('api/', include((router.urls, 'app_name'))),
	# 방식 2 (app_name은 path의 url 패턴의 기본 이름)
	path('api/', include((router.urls, 'app_name'), namespace='instance_name')),
	# 방식 3 (instance_name은 추가로 네임스페이스를 지정하는데 사용됨 (동일한 URL 패턴을 구분하기 위해 사용)
]

router가 아닌 아래 코드와 비교하기
app_name = 'accounts'
urlpatterns = [
    path('api/',user_logout,name='instance_name'),
]

```

```
urlpatterns += router.urls
# 위 방식 처럼 적용시킬 수도 있다.
```

위의 예는 다음 URL 패턴을 생성한다.

- URL 패턴: `^users/$` 이름:`'user-list'`
- URL 패턴: `^users/{pk}/$` 이름:`'user-detail'`
- URL 패턴: `^accounts/$` 이름:`'account-list'`
- URL 패턴: `^accounts/{pk}/$` 이름:`'account-detail'`

`register()` => 두 가지 인수를 필수적으로 가진다.
1. prefix : path에 보여질 URL 접두사
2. viewset : viewset class
선택적으로 basename을 설정할 수 있는데 이를 설정하게 되면 url이름을 직접 설정할 수 있다. 또한 queryset 속성이 없는 경우에는 basename을 설정해주어야 한다.
basename이 설정되어 있다면 prefix는 무시된다.
만약 viewset에서 반환하는 queryset이 없을 경우 basename이 꼭 있어야 한다.


추가 작업을 위한 라우팅

```
from myapp.permissions import IsAdminOrIsSelf
from rest_framework.decorators import action

class UserViewSet(ModelViewSet):
    ...

    @action(methods=['post'], detail=True, permission_classes=[IsAdminOrIsSelf])
    def set_password(self, request, pk=None):
```

- URL 패턴:`^users/{pk}/set_password/$`
detail=true로 인해 {pk} 값을 받아 특정한 객체만 수정하도록 한다.
- URL 이름:`'user-set-password'`

# Router의 종류

## SimpleRouter
ViewSet 클래스에 대해 기본적인 CRUD를 기본적으로 제공하는 router
`ViewSet` 클래스를 사용하여 뷰를 정의하고, SimpleRouter를 라우터로 설정하면 자동으로 URL 패턴 및 뷰와의 매핑을 생성한다.
추가적인 커스텀 라우팅이 필요하지 않은 경우에 적합하다.

## DefaultRouter
simplerouter의 확장으로 기본적인 API root가 들어가며, 사용자 등록(sign up), 로그인 라우팅도 제공한다.
일반적으로 인증과 관련된 엔드포인트를 자동으로 생성하고자 할 때 유용하다.

## CustomRouter
개발자가 직접 커스텀 라우팅을 정의할 수 있는 유연한 라우터
register() 메서드를 사용하여 원하는 URL 패턴 및 뷰와의 매핑을 설정할 수 있다.








