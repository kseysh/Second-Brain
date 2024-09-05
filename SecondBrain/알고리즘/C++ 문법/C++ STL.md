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

# map