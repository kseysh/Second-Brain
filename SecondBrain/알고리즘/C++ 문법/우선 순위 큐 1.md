정렬된 값들에 삽입 삭제를 할 때 유용한 자료구조이다.

## 선언
```cpp
multiset<int> sets;
```

## 삽입
```cpp
sets.insert(value)
```

## 삭제
```cpp
sets.erase(--sets.end()); // 포인터를 이용해 삭제하는 법
sets.erase(sets.find(value)); // 값을 이용해 삭제하는 법
```

## 최대/최솟값 반환
```cpp
*sets.begin()
*--sets.end() // or *problems.rbegin()
```

## 이동 (iterator 관련 함수)
```cpp
auto it = next(sets.begin(), n/2); // 절반만큼 이동
auto next = prev(it); // it보다 한 칸 이전
advance(it, -3) // void 값으로 it가 변함
```
advance() 함수는 반복자와 거리 값을 인자로 받고, 반복자를 거리 값만큼 증가