## 1. CharField

- 제한 문자열 삽입
- 반드시 최대 길이를 max_length 옵션에 지정

```python
title = models.CharField(max_length=100,**option)
```

##  2. EmailField

- 이메일 주소 형태를 넣을 때 사용됨

```python
email = EmailField(max_length=254,**options)
```

- 이메일 형식에 맞지 않으면 맞춰 쓰라는 경고문을 확인할 수 있다.

[![image](https://user-images.githubusercontent.com/26649731/75733010-6d815b80-5d37-11ea-9fc7-322d1dc5a0c0.png)](https://user-images.githubusercontent.com/26649731/75733010-6d815b80-5d37-11ea-9fc7-322d1dc5a0c0.png)

## [](https://github.com/dkyou7/TIL/blob/master/%ED%8C%8C%EC%9D%B4%EC%8D%AC/Django/5.%20%5BDjango%5D%20Model%20%ED%95%84%EB%93%9C%ED%83%80%EC%9E%85%20%EC%A0%95%EB%A6%AC.md#3-urlfield)3. URLField

- URL 주소 형태를 넣을 때 사용된다.

```python
url = URLField(max_length=200,**options)
```

## [](https://github.com/dkyou7/TIL/blob/master/%ED%8C%8C%EC%9D%B4%EC%8D%AC/Django/5.%20%5BDjango%5D%20Model%20%ED%95%84%EB%93%9C%ED%83%80%EC%9E%85%20%EC%A0%95%EB%A6%AC.md#4-textfield)4. TextField()

- 텍스트 필드는 문자열과 비슷하게 보이지만, 대용량 문자열을 처리하는 필드이다.
- 캐릭터 필드보다는 DB 용량을 많이 잡아 먹겠지만, 크기 제한을 굳이 할 필요가 없다.

```python
post = models.TextField(**options)
```

## [](https://github.com/dkyou7/TIL/blob/master/%ED%8C%8C%EC%9D%B4%EC%8D%AC/Django/5.%20%5BDjango%5D%20Model%20%ED%95%84%EB%93%9C%ED%83%80%EC%9E%85%20%EC%A0%95%EB%A6%AC.md#5-integerfield)5. IntegerField

- 32비트의 정수형 필드
- 사용 방법은 완전히 똑같지만 정수 사이즈에 따라 BigIntegerField, SmallIntegerField 사용가능
- default는 맨 처음 수정없이 저장될 경우의 값

```python
count = models.IntegerField(default=0, **options)
```

## [](https://github.com/dkyou7/TIL/blob/master/%ED%8C%8C%EC%9D%B4%EC%8D%AC/Django/5.%20%5BDjango%5D%20Model%20%ED%95%84%EB%93%9C%ED%83%80%EC%9E%85%20%EC%A0%95%EB%A6%AC.md#6-booleanfield)6. BooleanField

- True 혹은 False만 저장하는 필드
- Null 을 허용하기 위해서는 NullBooleanField를 사용한다.
- initial는 맨 처음 수정없이 저장될 경우의 값을 뜻한다.

```python
bool = models.BooleanField(initial=True, **options)
```

## [](https://github.com/dkyou7/TIL/blob/master/%ED%8C%8C%EC%9D%B4%EC%8D%AC/Django/5.%20%5BDjango%5D%20Model%20%ED%95%84%EB%93%9C%ED%83%80%EC%9E%85%20%EC%A0%95%EB%A6%AC.md#7-datetimefield)7. DatetimeField

- 시간과 관련된 값을 저장하는 필드
- DateField : 날짜만 저장하고 싶을 경우에 사용
- TimeField : 시간만 가질 경우 사용

```python
time = models.DateTimeField(auto_now=False, **options)
```

- 만약 자동으로 지금 시간대를 박아넣고 싶다면 auto_now = True로 옵션을 바꿔주면 된다.
- auto_now 옵션 : save 될 때마다 현재 시간을 갱신
- auto_now_add 옵션 : 맨 처음 생성한 현재 날짜만 갱신한다.

## [](https://github.com/dkyou7/TIL/blob/master/%ED%8C%8C%EC%9D%B4%EC%8D%AC/Django/5.%20%5BDjango%5D%20Model%20%ED%95%84%EB%93%9C%ED%83%80%EC%9E%85%20%EC%A0%95%EB%A6%AC.md#8-decimalfield)8. DecimalField

- 데시멀 필드는 소수점 관련 필드를 말한다.
- 이 필드는 max_digits와 decimal_places를 필수로 지정해야 한다.
- 표현할 수 있는 숫자의 수(max_digits)와 소수점 위치(decimal_places)를 지정함으로 지정된 숫자가 맞는지 체크가능하다.

```python
decimal = models.DecimalField(max_digits=None, decimal_places=None, **options)

# 예를 들어 max_digits=3, decimal_places=1의 값일 경우, 11.2가 예가 될 수가 있다.
```

## [](https://github.com/dkyou7/TIL/blob/master/%ED%8C%8C%EC%9D%B4%EC%8D%AC/Django/5.%20%5BDjango%5D%20Model%20%ED%95%84%EB%93%9C%ED%83%80%EC%9E%85%20%EC%A0%95%EB%A6%AC.md#9-filefield)9. FileField

- 파일을 업로드하는 필드
- upload_to 옵션에 반드시 해당 경로를 지정해야 한다.
- 폴더의 탐색은 사전에 settings.py에서 설정해놓았던 **MEDIA_ROOT** 경로부터 시작한다.

```python
# file will be uploaded to MEDIA_ROOT/uploads    
upload = models.FileField(upload_to='uploads/', **options)
```

## [](https://github.com/dkyou7/TIL/blob/master/%ED%8C%8C%EC%9D%B4%EC%8D%AC/Django/5.%20%5BDjango%5D%20Model%20%ED%95%84%EB%93%9C%ED%83%80%EC%9E%85%20%EC%A0%95%EB%A6%AC.md#10-imagefield)10. ImageField

- 파일 필드의 파생 클래스
- 해당 파일이 이미지인지를 체크해준다.

```python
img = models.ImageField(upload_to='images/', **options)
```

<hr>

[[Fields option]]
[[Django]]





