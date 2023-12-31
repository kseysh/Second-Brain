인자 전달 방식에는 대표적으로 
* [[Call by value]]
* [[Call by reference]]
방식이 있다.

그렇다면 과연 파이썬은 어떤 인자 전달 방식을 사용할까?

> 파이썬은 특이한 인자 전달 방식을 사용한다!

## 파이썬의 인자 전달 방식 - Pass By Assignment


파이썬은 모든 것이 객체로 정의 되며 객체의 종류는 두 가지로 나뉘어 진다.

* Mutable Object ( 불변 객체 ) : list, dict, set 등의 객체
* Immutable Object ( 가변 객체 ) : str, int, tuple 등의 객체

쉽게 말하면 mutable 객체는 pass by value 방식으로,
immutable 객체는 pass by reference 방식으로 구분된다.

### Mutable Object의 인자 전달 예시
```
def func(arg):
	arg += 1
	print(arg)

value = 2
func(value)

print(value)
```
func 안에서의 print 값은 3이 나오지만, func 밖에서의 print 값은 2가 전달된다.
즉, 전달한 인자의 값이 변경되지 않는 pass by value 방식과 같다.

### Immutable Object의 인자 전달 예시
```
def func(arr):
	arr[0] += 1
	print(arr)

a = [2]
func(a)

print(a)

```
이 예시에서는 func 안에서의 arr\[0] 값이 3으로 출력되고,
func 밖에서 인자로 전달했던 a\[0] 값도 원래 2에서 func를 거친 후 3으로 변경되게 된다.
즉 리스트의 값을 직접 변경한 pass by reference의 방식과 같다.

```
def func(arr):
	arr[0] = [1]
	print(arr)

a = [2]
func(a)

print(a)

```
하지만 위 예시처럼 리스트의 값을 변경하는 것이 아닌 다른 리스트로 지정해 주는 것이라면, 호출되고 나서도 유지가 된다. 따라서 func 안에서의 print를 통해 출력된 값은 \[1]이 되며, 
func 밖에서 print를 통해 출력된 값은 \[2]가 된다. 


## 왜 파이썬에서는 mutable과 immutable의 방식이 다를까?

그 이유는 이 전에도 언급했듯이, 파이썬에서는 **모든 것이 객체**이기 때문이다.
그래서 int 타입의 객체(immutable 객체)를 인자 값으로 넘기면 이 객체는 불변 객체이기 때문에 함수 안에서 새로운 값을 생성하는 방식을 사용한다. 
따라서 immutable 객체의 값 전달은 pass by value 방식으로 이루어지게 된다. 이런 방식을 택하게 되면 immutable 객체의 값을 복사하여 전달하는 것을 의미하므로 원본 객체와 전달된 객체는 독립적으로 동작하며, 한 쪽에서의 변경이 다른 쪽에 영향을 주지 않는다.

하지만 list 타입의 객체(mutable 객체)는 가변 객체이므로 새로 값을 만드는 것이 아니라 레퍼런스만 유지되기 때문에 call by reference 처럼 보이게 된다. 가변 객첸는 원본 객체 자체를 전달하고, 변수는 해당 객체를 가리키는 참조를 저장하므로 mutable 객체의 값 전달은 pass by reference 방식으로 이루어지게 된다.

이러한 차이를 만든 이유는 메모리 관리와 성능을 고려한 설계를 하기 위함이며,  immutable 객체는 객체가 변경되지 않는다는 보장을 제공하여 객체의 불변성을 유지할 수 있도록, mutable 객체는 객체의 참조를 전달함으로서 메모리 사용량을 줄이고, 객체의 변경이 필요한 경우에 효율적으로 수행할 수 있도록 도와줍니다. 따라서 파이썬에서 값 전달 방식은 객체의 특성에 따라 다르게 동작하며, 이는 객체의 불변성과 메모리 관리를 고려한 설계 원칙에 의한 것이라고 볼 수 있다.







