**Dictionary** : Map과 비슷하지만 중복을 허용한다.

**Maps**  : 검색 가능한 key - value 쌍
main operation : 검색 삽입 삭제
==중복을 허용하지 않는다.==

## Map ADT

**find()**
key를 *lookup* 한다.

**put()**
> 왜 put은 내부적으로 find를 수행하는가?
> > 중복을 허용하지 않기 위함이다.

**erase()**

<hr>

list-based map으로 표현하기 위해 unsorted list를 사용한다.

아래 수도코드에서 p는 pointer를 뜻한다.

**find()** in List-based Map
```
Algorithm find(k):
	for each p in [S.begin(),S.end()] do
		if p->key() = k then
			return p
	return S.end()
```
O(n) Time
찾으면서 최악의 경우 S를 전부 순회할 수도 있으므로

**put()** in List-based Map
```
Algorithm put(k,v):
	for each p in [S.begin(),S.end()] do
		if p->key() = k then
			p->setValue(v)
			return p
p = S.insertBack((k,v))
n = n + 1
return p
```
O(n) Time
일단 내부적으로 find함수를 사용하여 O(n) time이 소요된다.

	 setValue(v)를 사용해 k에 해당하는 항목이 이미 존재하는 경우, setValue를 이용해 update를 수행한다.

**erase()** in List-based Map
```
Algorithm erase(k):
	for each p in [S.begin(),S.end()] do
		if p.key() = k then
			S.erase(p)
			n = n - 1
```
O(n) Time
put함수와 같이 내부적으로 find함수를 사용하여 O(n) time이 소요된다.

이외의 ADT
size()
empty()
entrySet() : map에 있는 entry(key, value set)들을 반환
keySet() : map에 있는 key들 반환
values() : map에 있는 value들 반환


> unsorted list로 구현한 map은 search와 removal을 거의 활용하지 않고 put을 주로 사용하는 Maps에서 유용하다.
> why? 
> 중복을 확인하는 연산이 없다면 리스트의 끝에 새로운 요소를 추가하기만 하면 되어 O(1)의 시간 복잡도를 지니기 때문이다.





