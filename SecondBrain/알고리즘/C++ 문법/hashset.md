hash를 특정 타입에 대해 특수화하기 위한 코드
아래는 pair<int, int>에 대해 특수화한 코드이다.
pair가 들어간 부분에 원하는 구조체를 넣을 수도 있다.
```cpp
namespace std {
    template <>
    struct hash<pair<int, int>> {
        size_t operator()(const pair<int, int> &v) const {
            return v.first * 31 + v.second; // 소수(31)를 사용한 해시 결합
        }
    };
}
```
소수를 곱해서 결합하여 해시 충돌을 줄인 모습이다.