
## cbv를 사용할 때
```
class ~~~
order = openapi.Parameter('order', openapi.IN_QUERY, description='review_count / lower_price / higher_price / score / 입력값이 없을 시 일반 ', required=True, type=openapi.TYPE_STRING)
@swagger_auto_schema(tags=['마켓의 전체 리스트를 불러옵니다.'],manual_parameters=[order], responses={200: 'Success'})
def ~~~
```
맨 처음 값 'order' => 함께 전송할 이름
openapi.IN_QUERY => QUERYSTRING으로 값을 전송할 것인지 
openapi.IN_PATH => PATH 식으로 값을 전송할 것인지
description => swagger 문서 내에서 입력될 값
required =True => 값이 필요하다면 True
type=openapi.TYPE_STRING => 스트링 값 입력
type=openapi.TYPE_INTEGER => INT 값 입력


## fbv를 사용할 때
```
@swagger_auto_schema(
    method='get',
    operation_id='현재 주가 확인 및 사용자 투자종목 업데이트',
    operation_description='api호출한 순간의 해당종목 데이터',
    tags=['주식 데이터'],
    manual_parameters=[
        openapi.Parameter('symbol', in_=openapi.IN_QUERY, description='종목코드', type=openapi.TYPE_STRING),
    ],
)
@api_view(['GET'])
def now_data(request):
```