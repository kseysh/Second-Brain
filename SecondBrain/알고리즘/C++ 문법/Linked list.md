## 선언
```cpp
#include <list>

list<int> l1(5); // int list 생성 후 5개의 원소를 5로 초기화
list<int> l2(5, 1); // int list 생성 후 5개의 원소를 1로 초기화
list<int> l2 = {3, 7}; // int list 생성 후 3, 7로 초기화

```

## 앞 뒤에 값 넣기
```cpp
l.push_front(5);
l.push_back(5);
l.pop_front();
l.pop_back();
```

## Iterator
![[Iterator]]

## 특정 위치에 값 넣기
```cpp
l.insert(iter, 0); // iterator가 가리키는 위치에 0을 넣는다.
iter = l.erase(iter); // iterator가 가리키는 위치를 삭제하고 그 다음 원소의 위치를 반환한다.
```