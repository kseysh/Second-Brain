#### Query string과 Path variable은 각각 언제 쓰면 좋은가?

일반적으로 우리가 어떤 자원(데이터)의 위치를 특정해서 보여줘야 할 경우 Path variable을 쓰고, 정렬하거나 필터해서 보여줘야 할 경우에 Query parameter를 쓴다.

ex)
```
/users # Fetch a list of users
/users?occupation=programer # Fetch a list of programer user
/users/123 # Fetch a user who has id 123
```

위의 방식으로 우리는 어디에 어떤 데이터(명사)를 요청하는 것인지 명확하게 정의할 수 있다. 하지만, 그 데이터를 가지고 뭘 하자는 것인지 동사는 빠져있다. 그 동사 역할을 하는 것이 GET, POST, PUT, DELETE 메소드이다.

즉, Query string과 Path variable이 이들 메소드와 결합함으로써 "특정 데이터"에 대한 CRUD 프로세스를 추가의 엔드포인트 없이 완결 지울 수 있게 되는 것인다.  
(가령, `users/create` 혹은 `users?action=create`를 굳이 명시해 줄 필요가 없다.)

물론 위와 같은 규칙을 지키지 않더라도 잘 돌아가는 API를 만들 수 있다. 하지만 지키지 않을 경우 서비스 엔드포인트는 복잡해 지고, 개발자간/외부와 커뮤니케이션 코스트가 높아져 큰 잠재적 손실을 초래할 수 있으니 이 규칙은 잘 지켜서 사용하는 것이 필수라 하겠다.

출처 : https://velog.io/@jcinsh/Query-string-path-variable