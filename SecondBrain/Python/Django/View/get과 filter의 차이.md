즉, `get()`은 조건에 해당하는 object, 1개의 row를 반환한다.  
반면 `filter()`는 조건에 해당하는 여러 row를 모두 묶어 QuerySet이라는 형태로 반환한다.

### 그래서 get은, filter와 달리 여러 에러를 raise한다

위에서 서술한 차이 때문에, `get()` 은 2가지 에러를 발생시킨다.

첫 번째, `DoesNotExist` 에러다.  
`get()`은 조건에 해당하는 row가 존재하지 않는 경우, 'matching query does not exist' 라며 에러를 뱉는다.

반면 `filter()`는 에러가 아닌 빈 QuerySet을 반환한다.

두 번째, `MultipleObjectsReturned` 에러다.  
`get()`은 한 개의 row만 반환한다. 따라서 해당 조건에 해당하는 데이터가 여러개 일 경우 역시 에러를 뱉는다.

만약 `filter()`로 조회하면, 2개 row를 모두 QuerySet에 묶어 반환할 것이다.

```python
req_m = Member.objects.filter(id=10)

if not req_m:
    return JsonResponse({"message":"FAIL"}) 
else:
	req_m = req_m[0]
    return JsonResponse({"message":req_m.sns_id})
```

`Member` 테이블에 id=10인 row는 없다. 그리고 id=10인 멤버의 유무에 따라 다른 JsonResponse를 보내야 한다고 가정해보자.

처음부터 filter대신 `Member.objects.get(id=10)` 로 작성하면 더 편할 수 있겠으나, 이렇게 작성하면 바로 에러가 난다.

그래서 먼저 `filter()` 를 통해 존재 유무를 판단하고 > 있으면 해당 row의 sns_id값을 반환, 없으면 `FAIL`이라는 메세지를 반환하게 하는 것이다.

그래서 만약 OR 연산으로 조건을 걸고 싶다면 `Q객체`를 활용해야 한다.

- filter(condition1, condition2) > 조건 2개가 AND로 묶임
- filter(Q(condition1) | Q(condition2)) > 조건 2개가 OR로 묶임
- filter(Q(condition1) & Q(condition2)) > 조건 2개가 AND로 묶임
- Queryset1 & Queryset2 > 조건 2개가 AND로 묶임
    - 예시는 아래와 같다
```
User.objects.filter(first_name__startswith='R
& User.objects.filter(last_name__startswith='D')
```
        
        




https://velog.io/@mquat/django-get%EA%B3%BC-filter