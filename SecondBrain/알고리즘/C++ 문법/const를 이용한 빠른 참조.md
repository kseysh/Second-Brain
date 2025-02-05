
## 값 복사 (Pass by Value)
```cpp
vector<int> arr = {1, 2, 3, 4, 5};

for (auto val: arr) { 
    val += 10;  // val은 복사본이므로 원본 arr은 변경되지 않음
    cout << val << " ";
}
cout << endl;

// 출력: 11 12 13 14 15
// 원본 arr은 그대로 {1, 2, 3, 4, 5}
```
원본 배열의 값을 복사해서 사용하기 때문에, 루프 안에서 값을 변경해도 원본 arr에 영향이 없다.
하지만, 복사 비용이 존재한다.
## 읽기 전용 참조
```cpp
vector<int> arr = {1, 2, 3, 4, 5};

for (auto const &val: arr) { 
    cout << val << " ";
}
cout << endl;

// 출력: 1 2 3 4 5
// 원본 arr 그대로 유지됨
```
원본을 복사하지 않고 직접 참조하기 때문에 메모리를 낭비하지 않고 빠르다.