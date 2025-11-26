subscribe를 시작하기 전까지는 데이터 흐름이 시작되지 않음


## subscribe 과정
- onSubscribe() – 스트림을 구독할 때 호출
- request(unbounded) – subscribe를 호출하면 내부적으로 Subscription 이 생성됩니다 . 이 Subscription은 스트림에서 요소를 요청합니다. 이 경우 기본적으로 unbounded 로 설정되어 사용 가능한 모든 요소를 ​​요청합니다.
- onNext() – 이것은 모든 단일 요소에 대해 호출
- onComplete() – 마지막 요소를 받은 후 마지막으로 호출
- onError() - 에러가 발생하였을 때 호출