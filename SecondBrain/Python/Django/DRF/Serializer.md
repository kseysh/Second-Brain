> Serializer를 사용하는 이유?
> 복잡한 데이터를 JSON OR XML 형식으로 변환해주기 위해서( 직렬화 )

Serializer는 form / modelform 과 비슷하다.

## Serializer 선언

ex) models.py
```
from datetime import datetime

class Comment:
    def __init__(self, email, content, created=None):
        self.email = email
        self.content = content
        self.created = created or datetime.now()

comment = Comment(email='leila@example.com', content='foo bar')
```
위는 serializer를 위한 models.py 예제

Comment 객체에 해당하는 데이터를 직렬화 및 역직렬화하는데 사용하는 직렬 변환기를 선언한다.

ex) serializer.py
```
from rest_framework import serializers

class CommentSerializer(serializers.Serializer):
    email = serializers.EmailField()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()
```


## 객체의 직렬화

```
serializer = CommentSerializer(comment)
serializer.data
# {'email': 'leila@example.com', 'content': 'foo bar', 'created': '2016-01-27T15:17:10.375877'}
```
위처럼 CommentSerializer를 comment를 직렬화하는데 사용할 수 있다.
serializer.data를 통해 모델 인스턴스를 python 기본 데이터 유형으로 변환할 수 있다. 즉, comment 객체를 딕셔너리 형태로 얻을 수 있다.

```
from rest_framework.renderers import JSONRenderer

json = JSONRenderer().render(serializer.data)
json
# b'{"email":"leila@example.com","content":"foo bar","created":"2016-01-27T15:17:10.375877"}'
```
이는 데이터를 JSON으로 렌더링하여 직렬화를 수행할 수 있으며, JSONRender를 사용하여 데이터를 JSON 형식으로 변환한다.
최종적으로, serializer.data를 JSON으로 변환하여 직렬화된 데이터를 얻을 수 있다. 이 데이터는 네트워크를 통해 전송하거나 다른 시스템과 상호작용할 때 유용하게 사용될 수 있다.

## 객체의 역직렬화

역직렬화도 비슷하다. 먼저 stream을 python 기본 데이터 유형으로 파싱한다.
```
import io
from rest_framework.parsers import JSONParser

stream = io.BytesIO(json)
data = JSONParser().parse(stream)
```

이후 이러한 기본 데이터 유형을 validated_data의 dictionary 형태로 복원한다.
```
serializer = CommentSerializer(data=data)
serializer.is_valid()
# True
serializer.validated_data
# {'content': 'foo bar', 'email': 'leila@example.com', 'created': datetime.datetime(2012, 08, 22, 16, 20, 09, 822243)}
```

## Serializer에서 인스턴스의 저장


```
def create(self, validated_data):
        return Comment.objects.create(**validated_data)

    def update(self, instance, validated_data):
        instance.email = validated_data.get('email', instance.email)
        instance.content = validated_data.get('content', instance.content)
        instance.created = validated_data.get('created', instance.created)
        instance.save()
        return instance
```
`create()`메서드는 validated_data를 이용해 Comment 모델의 새로운 인스턴스를 생성하고 데이터베이스에 저장한다.
`update()` 메서드도 validated_data를 이용해 인스턴스의 필드 값을 업데이트하고, save() 메서드를 호출하여 변경사항을 데이터베이스에 저장한다.

추가로, .save() 호출 시 추가적인 데이터를 전달할 수도 있다. 예를 들어, 현재 사용자, 현재 시간 등과 같은 요청 데이터에 포함되지 않는 추가 데이터를 .save() 호출 시 전달할 수 있다.

```
serializer.save(owner=request.user)
```

## ModelSerializer

ex)
```
class AccountSerializer(serializers.ModelSerializer):
    class Meta:
        model = Account
        fields = ['id', 'account_name', 'users', 'created']
		fields = '__all__'

```
`model = Account `=> 모델 지정해주기
`fields = ['id', 'account_name', 'users', 'created']` => 직렬화 해줄 필드 가지고 오기
`fields = '__all__'` 이렇게 쓰면 모든 필드를 가지고 올 수 있다.

`exclude = ['users']`  이렇게 사용해서 제외할 수도 있다.
`depth = 1`  이렇게 하면 model 안의 클래스를 가져올 수도 있다.

```
class AccountSerializer(serializers.ModelSerializer):
    url = serializers.CharField(source='get_absolute_url', read_only=True)
    groups = serializers.PrimaryKeyRelatedField(many=True)

    class Meta:
        model = Account
        fields = ['url', 'groups']
```

위 코드처럼 추가적인 필드를 지정해줄 수 있다.







