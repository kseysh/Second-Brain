[[Fields 종류]]
# Fields options

## null
Default : false
True이면 데이터베이스에 null값을 저장한다.

## blank
Default : false
blank 값을 허용한다. but null과는 다른 개념이다 "" 와 null의 차이인듯? (document에는 null은 데이터베이스에 관련된 개념이며, blank는 유효성에 관련된 개념이라고 소개되어 있다.)

## choices
text field 대신 select box를 생성한다.
``` 예제
class Person(models.Model):
    SHIRT_SIZES = [
        ("S", "Small"),
        ("M", "Medium"),
        ("L", "Large"),
    ]
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```

서로 다른 두 개의 사용 예제

```
class Runner(models.Model):
    MedalType = models.TextChoices("MedalType", "GOLD SILVER BRONZE")
    name = models.CharField(max_length=60)
    medal = models.CharField(blank=True, choices=MedalType.choices, max_length=10)
```

## default
필드의 기본값입니다. 이것은 값 또는 콜러블 객체일 수 있습니다. 호출 가능하면 새 객체가 생성될 때마다 호출됩니다.

## primary key
True인 경우 모델의 primary key로 데이터베이스를 인식하기 위한 key이다. Django는 primary key를 보유하기 위해 자동으로 추가하므로 primary key를 재정의하는 경우가 아니면 IntegerField를 설정할
필요는 없다.

사용자 정의 primary key를 지정하는 예
```
id = models.BigAutoField(primary_key=True)
```
## unique
True인 경우 이 값은 테이블 전체에서 고유해야 한다

## Verbose_name
verbose name이 주어지지 않으면 자동적으로 field의 추상 이름을 생성한다.

In this example, the verbose name is `"person's first name"`:
```
first_name = models.CharField("person's first name", max_length=30)
```
In this example, the verbose name is `"first name"`:
```
first_name = models.CharField(max_length=30)
```

	verbose_name은 대문자로 표기하지 않는다.

`foreignKey`, `ManyToManyField` and `OneToOneField` 는 첫 매개변수로 model의 class를 요구한다. 그래서 verbose_name을 keyword 매개변수로 이용한다.

예제
```
poll = models.ForeignKey(
    Poll,
    on_delete=models.CASCADE,
    verbose_name="the related poll",
)
sites = models.ManyToManyField(Site, verbose_name="list of sites")
place = models.OneToOneField(
    Place,
    on_delete=models.CASCADE,
    verbose_name="related place",
)
```

<hr>

# Relationships

## ForeignKey (Many to one)
예제
```
class Car(models.Model):
    manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE)
```
`on_delete=models.CASCADE` => Manufacture가 삭제되면 그에 연관된 Car도 삭제하겠다는 의미

## ManyToManyField
예제
```
class Pizza(models.Model):
    toppings = models.ManyToManyField(Topping)
```
여기서 여러가지 토핑은 여러가지 피자 위에 올라갈 수 있다.
또한 ManyToManyField의 이름은 (여기서는 toppings) 모델 오브젝트들의 관계를 복수로 (s를 붙혀) 표시하는 것이 좋다.

Person과 Group은 ManyToMany 관계를 나타내지만, 개인이 그룹에 가입한 날짜 같이 얻으려는 세부정보가 추가적으로 있다면, 다대다 관계를 관리하는 데 사용할 모델을 지정하고 (Membership) 그 후 중간 모델에 추가 필드를 넣을 수 있다.  이렇게 하면 ForeignKey를 사용하면서 ManyToMany 관계를 보여줄 수 있다.

```
class Person(models.Model):
    name = models.CharField(max_length=128)

    def __str__(self):
        return self.name

class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(Person, through="Membership")

    def __str__(self):
        return self.name

class Membership(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    date_joined = models.DateField()
    invite_reason = models.CharField(max_length=64)
```

## OneToOneField
모델 간의 일대일 관계를 정의하는 필드.
예를 들어, 사용자(User) 모델과 사용자 프로필(UserProfile) 모델 간에는 일대일 관계가 성립한다.
```
class UserProfile(models.Model): 
user = models.OneToOneField(User, on_delete=models.CASCADE)
```

<hr>

# Meta
데이터베이스 테이블 이름, 정렬 순서, 복수형 표현, 제약 조건 등을 모델의 Meta 클래스 내에서 지정할 수 있다.

Meta 데이터는 일반적으로 데이터의 특성, 구조, 관계 등을 설명하고 정의하는 데 사용된다.

`Options.db_table`
=> 모델에 사용할 데이터베이스 테이블의 이름:
```
db_table = "music_album"
```

`Options.ordering`
=> 객체 목록을 가져올 때 사용하기 위한 객체의 기본 순서

`ordering = ["-order_date"]`
=> default, -는 내림차순을 나타낸다. -가 없다면 오름차순으로 정렬된다.

`ordering = ["pub_date"]`
=> 이렇게 작성하면 pub_date를 오름차순으로 정렬할 수 있다.

`ordering = ["-pub_date", "author"]`
=> pub_date를 내림차순으로 ordering한 후 author를 오름차순으로 ordering하려면 위 처럼 하면 된다.

## 추상 기반 클래스
추상 기본 클래스는 일부 공통 정보를 여러 다른 모델에 입력하려는 경우에 유용하다. 기본 클래스를 작성하고 Meta class에 `abstract=True` 클래스에 넣는다면 이 모델은 데이터베이스 테이블을 만드는 데 사용되지 않고 다른 모델의 기본 클래스로 사용될 때 해당 필드가 하위 클래스의 필드에 추가된다.

ex) 
```
class CommonInfo(models.Model):
    name = models.CharField(max_length=100)
    age = models.PositiveIntegerField()

    class Meta:
        abstract = True

class Student(CommonInfo):
    home_group = models.CharField(max_length=5)
```

CommonInfo에 대한 database table은 만들어지지 않지만, CommonInfo는 Student에 상속해주는 용도로 사용된다.

## Meta 상속
자식 클래스가 자체 Meta 클래스를 선언하지 않으면 부모의 Meta 클래스를 상속한다. 자식이 부모의 메타 클래스를 확장하려는 경우 자식 클래스에서 Meta 클래스를 하위 클래스로 만들 수 있다.

ex)
```
class CommonInfo(models.Model):
    name = models.CharField(max_length=100)
    age = models.PositiveIntegerField()

    class Meta:
        abstract = True
        ordering = ["name"]

class Unmanaged(models.Model):
    class Meta:
        abstract = True
        managed = False

class Student(CommonInfo, Unmanaged):
    home_group = models.CharField(max_length=5)

    class Meta(CommonInfo.Meta, Unmanaged.Meta):
        pass
```
CommonInfo 클래스 : 
- Meta 클래스에서 `abstract = True`로 설정되어 있으므로, CommonInfo 클래스는 실제로 데이터베이스에 테이블로 생성되지 않습니다.
- Meta 클래스에서 `ordering = ["name"]`로 설정되어 있으므로, CommonInfo 클래스를 쿼리할 때 `name` 필드를 기준으로 정렬됩니다.
Unmanaged 클래스:
* Meta 클래스에서 `managed = False`로 설정되어 있으므로, Django는 Unmanaged 클래스에 대한 데이터베이스 관리 작업(테이블 생성, 변경, 삭제 등)을 수행하지 않습니다.
	> 보통 외부 데이터 소스나 기존 데이터베이스 스키마를 사용하는 특수한 경우에 사용

Student 클래스:
* pass 문은 추가적인 설정을 하지 않고 기본적으로 상속받은 Meta 클래스의 설정을 사용한다는 의미입니다.

<hr>

# model methods

> model method를 사용하는 이유 : 
> model method를 사용해서 모델의 동작과 관련된 로직(데이터베이스와 관련된 동작)은 model에서, view와 관련한 로직(사용자 요청에 대한 처리)은  view에서 처리하기 위함

	model method를 정의할 때 항상 `def do_something(self):` 처럼 self를 매개변수로 항상 넣어준다.

## \_\_str\_\_
모든 객체의 문자열 표현을 반환하는 함수.
대화형 콘솔이나 관리자에 객체를 표시할 때 보여지는 이름을 변경한다.  장고에서 제공하는 기본값을 객체를 인식하는데 도움이 되지 않을 확률이 높으므로 이 메서드를 정의하여 각 객체를 인식할 수 있도록 하는 것이 좋다.


## 필드명 제한
Python 예약어이면 안된다.
하나 이상의 underscore( \_ )를 포함하지 않는다.
underscore로 필드명을 끝내지 않는다.