# array 

# vector 
- itr = container.begin();
    - 첫번째 주소를 받아
- while (itr != container.end())
    - 반복자가 끝까지 다 돌때까지.
- \*itr
    - 반복자가 가리키는 원소를 이용한 후 (간접참조같이)
- ++itr 다음 반복자.

- 정보
    - capacity : 벡터 여유 공간 리턴
    - size() : 원소 개수 리턴
    - empty() : 빈 벡터 유무 bool 리턴
- 반복자
    - begin() : 벡터 첫 번째 원소
    - end() : 벡터 마지막 원소 다음
    - rbegin() : 역순, 벡터 마지막 원소
    - rend() : 역순, 벡터 첫번째 원소 이전
- 원소 접근
    -  \\\[\n\] : \[인덱스] 로 해당 원소 접근
    - front() : 첫번째 원소 접근
    - back() : 마지막 원소 접근
- 원소 추가
    - assign(반복자,반복자) : 호출한 벡터의 해당 반복자 구간으로 할당
    - assign(n, x) : x 를 n 번만큼 반복하여 집어 넣음
    - push_back(원소) : 원소를 벡터 끝에 추가
    - insert(반복자,원소) : 해당 반복자가 가리키는 곳에 원소 삽입
    - insert(반복자, n, x) : 해당 반복자가 가리키는 곳에 x 원소를 n 개 삽입
- 원소 삭제
    - pop_back() : 마지막 원소 제거
        - erase(반복자) : 해당 반복자가 가리키는 곳 원소 삭제
        - clear() : 벡터 원소 전부 삭제
- 원소 바꾸기
    - swap(다른 벡터) : 다른 벡터와 원소 바꿈
# stack

# queue

# deque

# list

# set
- begin() : 시작 iterator return
- end() : 끝 다음 iterator return
- insert(element) : insert
- erase(element) : erase
- clear() : element 전부 삭제
- find(element) : element 값에 해당되는 iterator return, 원소 없으면 end() 호출
- empty() : 비어 있으면 true, 아니면 false
- size() : 원소 개수 return
# map
- `[key]` 연산자 활용하여 value 값 return 가능. 단, 없는 key 값을 참조 시 자동으로 디포트 생성자 호출해서 원소를 추가하여 '0' 을 return 함. 그러므로 되도록이면 `find` 함수로 원소가 키에 존재하는지 확인 후에, 값 참조가 좋음.

- begin() : 시작 iterator return
- end() : 끝 바로 다음 iterator return
- insert(std::pair) : (key,value) 형태로 element 추가
    - `std::pair` 객체는 C++ 표준 객체
        
        ```cpp
        template <class T1, class T2>
        struct std::pair {
        T1 first;
        T2 second;
        };
        ```
        
    - `first` 는 key, `second` 는 value
    - 중복 원소 허용 X, 기존 Key 있으면 무시
- erase(key) : key 대응 element 삭제
- clear() : 모든 element 삭제
- find(key) :
    - key 에 대응되는 iterator return
        - key 값을 못 찾으면 end() 호출
- count(key) : key 값에 해당되는 원소 개수 return
- empty() : map 이 비어 있으면 true, 아니면 false
- size() : 원소 개수 return

```cpp
map<string,int> map;
map.insert(pair<string, int>("식빵",5));
```

# 시퀀스 사용 컨테이너 Case
- 일반적인 상황의 경우 `vector` 가 만능!
- 끝이 아닌 중간에 원소 추가/제거가 많고, 원소 순차 접근만 한다면 `list`
- 맨 처음, 끝에 원소 추가 작업이 많으면 `deque`


# algorithm
- `min_element(container.begin(),container.end())` : 범위에서 가장 작은 원소의 반복자를 return
- `max_element(container.begin(),container.end())` : 범위에서 가장 큰 원소의 반복자를 return
- `find(container.begin(),container.end(), item)` : item 검색해서 찾아서 그 반복자 return, 없으면 end() return
    
- 정렬
    - `sort(container.begin(),container.end())` : 범위를 정렬, 세 번재 인수로 정렬 기준이 되는 사용자 정의 비교 함수 (lambda) 포인터 넘길 수도 있음
    - `sort(container.begin(),container.end(), compare)` : 범위를 정렬, 세 번재 인수로 정렬 기준이 되는 사용자 정의 비교 함수 (lambda) 포인터 넘길 수도 있음
    - `stable_sort` : stable 정렬 (sort 는 unstable)
    - `partial_sort` : 배열 일부분만 정렬
- `reverse` : 인수로 넘긴 범위의 순서를 거꾸로 뒤집음.
    
- 수학 관련 :
    - max(a,b), min(a,b)
    - max_element(시작 주소/반복자, 끝 주소/반복자), min_element(시작 주소/반복자, 끝 주소/반복자)

- 원소 수정하지 않는 작업 :
    - all_of : 범위 안에 모든 원소들이 조건을 만족하는지 확인한다.
    - any_of : 범위 안의 원소들 중 조건을 만족하는 원소가 있는지 확인한다.
    - find : 범위 안에 원소들 중 값이 일치하는 원소를 찾는다.
    - find_if : 범위 안에 원소들 중 조건과 일치하는 원소를 찾는다.
    - count : 특정 값과 일치하는 원소의 개수를 센다.
    - count_if : 특정 조건을 만족하는 원소의 개수를 센다.
    - equal : 두 원소의 나열들이 일치하는지 확인한다.
    - search : 어떤 원소들의 나열을 검색한다.