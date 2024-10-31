## 오름차순 정렬
`algorithm` 헤더 include 후
`sort(arr, arr + arrSize)`

## 내림차순 정렬
`sort(arr, arr+3, greater<>());`

## 시간 복잡도
`nlogn`

## 벡터 정렬
- `sort(v.begin(), v.end()); ` 
- `sort(v.begin(), v.end(), compare);`                //사용자 정의 함수 사용  
- `sort(v.begin(), v.end(), greater<자료형>());`    //내림차순