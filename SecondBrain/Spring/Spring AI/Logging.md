SimpleLoggerAdvisor로 ChatClient의 요청 및 응답 데이터를 로그로 기억할 수 있다.

로깅을 활성화하려면 ChatClient를 생성할 때 SimpleLoggerAdvisor를 Advisor 체인에 추가해야 한다. 
이 Advisor는 체인의 마지막쯤에 추가하는 것이 권장된다.

```java
ChatResponse response = ChatClient.create(chatModel).prompt()
    .advisors(new SimpleLoggerAdvisor())
    .user("Tell me a joke?")
    .call()
    .chatResponse();
```

로그를 확인하려면 application.properties 또는 application.yml 파일에 다음과 같이 로깅 레벨을 설정
```java
logging.level.org.springframework.ai.chat.client.advisor=DEBUG
```