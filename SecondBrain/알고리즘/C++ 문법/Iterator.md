Iterator란?
=> 배열의 요소를 가리키는 포인터

```cpp
vector<int>::iterator iter;
v.begin(); // 배열의 첫 번째 주소 반환
v.end(); // 배열의 마지막 요소 바로 뒤, 즉, 쓸모 없는 값을 가리키는 주소를 반환한다.
```

## for문에서 사용하는 Iterator
```cpp
for(vector<int>::iterator iter = v.begin; iter != v.end(); iter++){
	cout << *iter;
}
```
