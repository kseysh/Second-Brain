divmod는 작은 숫자를 다룰 때는 a//b, a%b 보다 느립니다. 대신, 큰 숫자를 다룰 때는 전자가 후자보다 더 빠르지요.

파이썬의 int(x, base=10) 함수는 진법 변환을 지원합니다.

파이썬에서는 ljust, center, rjust와 같은 string의 메소드를 사용해 코드를 획기적으로 줄일 수 있습니다.

s = '가나다라'

n = 7

s.ljust(n) # 좌측 정렬

s.center(n) # 가운데 정렬

s.rjust(n) # 우측 정렬

파이썬에서는

파이썬은 이런 데이터를 상수(constants)로 정의해놓았습니다.

import string

string.ascii_lowercase # 소문자 a~z

string.ascii_uppercase # 대문자 A~Z

string.ascii_letters# 대소문자 모두 a~Z

string.digits # 숫자 0~9

파이썬의 sort() 함수를 사용하면 리스트의 원소를 정렬할 수 있습니다. 이때, sort 함수는 원본의 멤버 순서를 변경하지요.

따라서 원본의 순서는 변경하지 않고, 정렬된 값을 구하려면 sort 함수를 사용할 수 없습니다. 파이썬의 sorted를 사용해보세요. 반복문이나, deepcopy 함수를 사용하지 않아도 새로운 정렬된 리스트를 구할 수 있습니다.

	파이썬의 Asterisk(*) 이해하기

1.곱셈 및 거듭제곱 연산으로 사용할 수 있다.

2. 리스트형 컨테이너 타ᅟᅵᆸ의 데이터를 반복 확장하고자 할 때 사용가능하다.

		ex)list=[1]*10

3.가변인자를 사용하고자 할 때, 컨테이너 타입의 데이터를 unpacking할때

파이썬에서는 인자의 종류가 2가지가 있는데 하나는 positional arguments이고, 하나는 keyword arguments이다. 전자는 말그대로 위치에 따라 정해지는 인자이며, 후자는 키워드를 가진 즉, 이름을 가진 인자를 말한다.

def sum(a,b)

return a+b

sum(1,2) =>3

t=(2,3)

sum(t) => error

	sum(*t) => 5

	def sum2(*args):

result=0

for i in args:

result += i

return result

sum2(1,2,3) => 6 (여러가지 인자를 넣어 사용가능)

sum(1,2,3) => error

d={‘a’:3,‘b’:4}

	sum(**d) => 7

	def person(**kwargs):

for k, v in kwargs.items():

print(k,v)

person(name=’transfer‘,age=19,’height‘:176)

=> name transfer

age 19

height 176

p={name=’transfer‘,age=22,’height‘:180}

person(p) => error

	person(**p)

=>name transfer

age 19

height 176

zip함수를 이용해 2차원 배열 뒤집기

파이썬의 zip과 unpacking 을 이용하면 코드 한 줄로 리스트를 뒤집을 수 있습니다.

	mylist = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

	new_list = list(map(list, zip(*mylist)))

zip함수 =>

사용 예 #1 - 여러 개의 Iterable 동시에 순회할 때 사용

	list1 = [1, 2, 3, 4]

	list2 = [100, 120, 30, 300]

	list3 = [392, 2, 33, 1]

answer = []

for number1, number2, number3 in zip(list1, list2, list3):

print(number1 + number2 + number3)

사용 예 #2 - Key 리스트와 Value 리스트로 딕셔너리 생성하기

	animals = ['cat', 'dog', 'lion']

	sounds = ['meow', 'woof', 'roar']

answer = dict(zip(animals, sounds)) # {'cat': 'meow', 'dog': 'woof', 'lion': 'roar'}

python에서는

파이썬의 zip을 이용하면 index를 사용하지 않고 각 원소에 접근할 수 있습니다.

def solution(mylist):

answer = []

	for number1, number2 in zip(mylist, mylist[1:]):

answer.append(abs(number1 - number2))

return answer

파이썬의 map을 사용하면 for 문을 사용하지 않고도 멤버의 타입을 일괄 변환할 수 있습니다.

	list1 = ['1', '100', '33']

list2 = list(map(int, list1))

파이썬의 str.join(iterable)을 사용하면 이 코드를 두 줄로 줄일 수 있습니다 .

	my_list = ['1', '100', '33']

answer = ''.join(my_list) => ‘110033’

파이썬에서는

itertools.product를 이용하면, for 문을 사용하지 않고도 곱집합을 구할 수 있습니다.

import itertools

iterable1 = 'ABCD'

iterable2 = 'xy'

iterable3 = '1234'

print(list(itertools.product(iterable1, iterable2, iterable3)))

파이썬의 다양한 기능을 사용하면, for 문을 사용하지 않고도 리스트를 이어 붙일 수 있습니다.

	my_list = [[1, 2], [3, 4], [5, 6]]

# 방법 1 - sum 함수

answer = sum(my_list, [])

# 방법 2 - itertools.chain

import itertools

list(itertools.chain.from_iterable(my_list))

# 방법 3 - itertools와 unpacking

import itertools

	list(itertools.chain(*my_list))

# 방법 4 - list comprehension 이용

	[element for array in my_list for element in array]

# 방법 5 - reduce 함수 이용 1

from functools import reduce

list(reduce(lambda x, y: x+y, my_list))

# 방법 6 - reduce 함수 이용 2

from functools import reduce

import operator

list(reduce(operator.add, my_list))

파이썬에서는

itertools.permutation를 이용하면, for문을 사용하지 않고도 순열을 구할 수 있습니다.

import itertools

	pool = ['A', 'B', 'C']

print(list(map(''.join, itertools.permutations(pool)))) # 3개의 원소로 순열 만들기

print(list(map(''.join, itertools.permutations(pool, 2)))) # 2개의 원소로 순열 만들기

가장 많이 등장하는 알파벳 찾기

파이썬의 collections.Counter 클래스를 사용하면 이 코드를 간략하게 만들 수 있습니다.

import collections

	my_list = [1, 2, 3, 4, 5, 6, 7, 8, 7, 9, 1, 2, 3, 3, 5, 2, 6, 8, 9, 0, 1, 1, 4, 7, 0]

answer = collections.Counter(my_list)

	print(answer[1]) # = 4

	print(answer[3]) # = 3

	print(answer[100]) # = 0

파이썬에서는

파이썬의 list comprehension을 사용하면 한 줄 안에 for 문과 if 문을 한 번에 처리할 수 있습니다.

	mylist = [3, 2, 6, 7]

	answer = [number**2 for number in mylist if number % 2 == 0]

보통 프로그래밍 언어에서 'else'라고 하면 if와 함께 오는 경우가 거의 대부분입니다.

하지만 파이썬에서는 for 문과도 함께 쓰기도 합니다.

for와 함께 쓰는 else는, for문이 중간에 break 등으로 끊기지 않고,

끝까지 수행 되었을 때 수행하는 코드를 담고 있습니다.

파이썬에서는 이진탐색코드를 그냥 사용가능

파이썬의 bisect.bisect 메소드를 사용하면 이 코드를 간략하게 만들 수 있습니다.

import bisect

	mylist = [1, 2, 3, 7, 9, 11, 33]

print(bisect.bisect(mylist, 3))

파이썬에서는

	파이썬에서는 __str__ 메소드를 사용해 class 내부에서 출력 format을 지정할 수 있습니다.

class Coord(object):

	def __init__ (self, x, y):

self.x, self.y = x, y

	def __str__ (self):

return '({}, {})'.format(self.x, self.y)

point = Coord(1, 2)

가장 큰 수 inf

음수 기호를 붙이는 것도 가능하다.