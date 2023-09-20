## Hash Function
Hash function h는 주어진 유형의 키를 고정된 간격의 정수에 매핑한다. 
	ex) `h(x) = x mod N`
h(x)는 x의 hash value라고 부른다.

<hr>

## Hash Functions
hash function은 두 단계로 나뉜다.

1. Hash Code
	* `h : keys -> integers`
2. Compression function (압축 함수)
	* `h : integers -> [ 0 , N-1 ]`

처음에 Hash Code가 적용되고, 이 후 Compression function이 적용된다.
**Hash Function의 목표**는 "key를 랜덤하게 여러 군데로 **분산** 시키는 것" 이다.

<hr>

## Hash Codes

hash code의 종류
* ### Integer cast
	데이터의 해시 코드를 정수형으로 변환하는 과정
	==int의 비트보다 작거나 같은 길이일 때 적합==하다.
* ### Component sum (요소 합)
	문자열의 각 문자에 대한 아스키 코드 값을 모두 더하여 해시 값을 생성하는 방법
	(강의노트에서는 고정된 길이의 비트로 나눈다고 되어있다.)
	==int보다 크거나 같은 bit의 숫자일 때 적합==하다.
* ### polynomial accumulation
	문자열의 각 문자를 하나의 숫자로 변환한 다음, 이들을 다항식의 계수로 사용하여 다항식을 만든 후, 이 다항식의 값을 계산하여 해시 값을 생성하는 방법
	특히 String에서 알맞는 방법
	ex) `h(x) = a*x^2 + b*x^1 + c*x^0`

<hr>

## Compression Functions
* ### Division
	`h(y) = y mod N`
	소수로 N을 구성하면 소수의 배수가 아닌 이상 균등하게 들어갈 수 있다.
	만약 6이 N이 된다면 약수인 2, 3, 6의 배수는 특정 공간에만 들어가기 때문이다.
	즉, N은 해시 충돌을 줄이기 위해 **소수이면서 가능한 큰 값**이 선호된다.
* ### Multiply, Add and Divide (MAD)
	`h(y) = (ay + b) mod N`
	a와 b는 양수여야 한다.
	`a mod N`은 0이 아니어야 한다. (그렇다면 모든 y가 b에 매핑될 것이므로)
	

<hr>

## Collision Handling
* ### Separate Chaining
	충돌이 발생한 버킷(bucket)에 연결 리스트(linked list)를 사용하여 충돌된 데이터를 연결하는 방법
	* 단점 : 간단하지만, 각 버킷마다 연결 리스트를 유지해야해 메모리 공간이 많이 필요하다
	* 충돌이 많이 발생하면 연결 리스트의 길이가 길어져 검색 시간이 증가할 수 있다.

	각 노드의 문을 열어 놓는 것과 비슷하여 `Open hashing` 이라고도 불린다.

	worst case : O(n)
	expected Time : O(1+α)
	
	데이터 삽입의 시간 복잡도는 O(1)
	평균적으로 한 bucket당 n/N개의 충돌이 있을 것이므로 연결리스트의 평균 길이는 α이다따라서 탐색 작업은 연결 리스트의 길이에 비례하여 O(α)의 시간 복잡도를 가지게 된다. 
	합해서 O(1+α)의 expected time을 지닌다.

	=> α ∈ O(1)이라면 expected Time은 O(1)이다.
	=> N ∈  θ(n)이라면 expected Time은 O(1)이다. (같은 말)

	bucket(연결 리스트)의 크기가 n이하이면 상수 time의 연산이 가능하다. (expected time에 한해서)

n : 해싱 된 요소의 수
N : bucket의 수

> Load Factor (α = n / N): 해시테이블에서 사용중인 bucket의 비율 
> 1에 가까울 수록 테이블이 가득 차 있다는 의미
> open addresing : 0.5, closed addressing : 1

#### Separate Chaining operations

```
Algorithm find(k):
	return A[h(k)].find(k)

Algorithm put(k,v):
	t = A[h(k)].put(k,v)
	if t=null then
		n = n + 1
	return t

Algorithm erase(k):
	t = A[h(k)].erase(k)
	if not t = null then
		n = n - 1
	return t
```
여기서 n은 해싱된 elements의 수,
N은 버킷의 수이다.

* ### Linear Probing
충돌한 요소들을 가능한 table cell의 뒤에 배치하여 충돌을 해결하는 방법. *환형 배열*을 사용하기도 한다. 
> open addressing/ closed hasing이라고도 불린다.

#### probe란?
각 테이블에 데이터가 들어있는지 확인하는 과정을 probe라 한다.

#### linear probing의 단점 : 
충돌하고 linear probing을 진행한 데이터들이 뭉쳐있을 수 있으며 이는 미래에 probe연산의 시간이 길어질 수 있다는 문제점이 있다. 뭉쳐진 데이터들을 Secondary Cluster라고 부르기도 한다.

#### linear probing의 수행 과정
![[linear probing.jpg]]

#### linear probing의 find함수
```
Algorithm find(k)
	i<-h(k)
	p<- 0
	while (p != N)
		c<-A[i]
		if c=∅
			return null
		else if c.key() = k
			return c.value()
		else
			i <- (i + 1) mod N
			p <- p + 1
	return null 
```

#### linear probing의 put(), delete()
>AVAILABLE : 해당 셀이 이전에 삭제된 데이터가 저장되었던 셀임을 나타내는 특수한 상태

##### put()의 과정
1. table이 full이면 예외 처리를 한다.
2. h(k)부터 조건을 만족 할 때까지 probe를 시작한다.
	* 조건 1 : 해당 셀이 비어있거나, AVAILABLE로 표시되어 있는 경우
	* N개의 셀이 연속적으로 probe 되었지만 삽입 가능한 셀을 찾지 못한 경우 => 다른 방법을 고려해 보아야한다.
3. (k, o)를 셀 i에 저장한다.

##### delete()의 과정
1. key가 k인 항목을 검색한다.
2. (k, o) 를 찾는다면, 그 장소에 AVAILABLE을 대신 넣어놓고, o를 반환 받는다.
3. 못 찾았다면 null을 반환한다.


* ### Quadratic Probing
해시테이블에서 충돌이 발생했을 때, 다음 위치를 찾기 위해 일정한 간격으로 이동하는 방식
충돌이 발생한 위치로부터 고정된 간격을 계산하고, 그 간격을 제곱한 값으로 다음 위치를 계산하는 방식을 통해, 충돌이 발생한 위치에서 더 멀리 떨어진 위치를 탐색하게 한다.

#### Quadratic Probing의 단점
수학적으로 계산했을 때, bucket의 절반밖에 사용하지 못한다.

#### Quadratic Probing의 수행과정
![[Quadratic probing.jpg|600]]
==5 => 5+1² => 5+2² => 5+3² ... 순으로 간다.==
**5 => 5+1² => 5+1²+2² 순으로 가는 것이 아니니 주의!**

>SUHA (simple Uniform Hashing Assumption) 란?
>>해시 함수가 임의의 키들을 균일하게 해시 테이블의 인덱스로 매핑시키는 것으로 가정하는 것
>>즉, 키가 등장할 확률이 모두 동일하고, 키 간의 독립성이 보장된다는 가정

* ### Double Hashing
해시 테이블에서 충돌이 발생했을 때 다른 해시 함수 `h'(k)` 를 이용하여 다음 위치를 찾는 방식
`h(k) + j*h'(k) mod N` => `h(k) + h'(k)`에 어떤 값이 있다면 `h(k) + 2*h'(k)`에 값을 넣는 방식이다
>따라서 `h'(k)`는 0이 나오면 안된다.

보통 secondary hash function을 `h'(k) =  q - (k mod q)`로 정의하여 1에서 q 사이의 range를 지니도록 한다.
>이 때 q < N 이고, q는 소수를 지닌다.

첫 해쉬 값 `h(k)` 두 번째 해쉬 값 `h(k)`+`h'(k)` 세 번째 해쉬 값 `h(k)`+`2*h'(k)` 이런 식으로 계산한다.
#### Double Hashing의 예시
![[더블 해싱.jpg|400]]
h(k)값을 secondary hash function에 넣는 것이 아닌, 새롭게 h'(k) 값을 구해서 h(k)와 더하는 방법임.

* ### rehashing
hash table이 거의 차게 되면 operation이 느려지게 된다. 따라서 rehashing을 통해 해시 테이블의 크기를 두 배로 확장하는 방식으로 기존의 해시 테이블에서 모든 요소를 새로운 크기의 해시테이블로 재배치한다.

> 두 배로 doubling을 하는 이유 :수학적으로 doubling을 하는 것이 데이터 1개를 추가하는데 O(1)Time이 걸리기 때문이다.

새로운 hash function을 만들어 재배치 한다. 있는 hash table을 계속 사용하게 되면 원래 있던 hash table쪽에 데이터가 뭉치는 현상이 발생하기 때문이다.

### Performance of Hashing
searches, insertions, removals => worst Time : O(n)
하지만 이는 모든 해싱 값이 같은 조건인 너무 극단적인 Case이다.

searches, insertions, removals => **N ∈ θ(n)** 이라면, expected Time : O(1) 

[[Dictionary & Maps]]
