LLM과 대화할 때, context를 유지하기 위해 사용하는 메시지들 (UserMessage + AssistantMessage)

Spring AI는 대화 기억을 위해 ChatMemory &  ChatMemoryRepository로 기능 분리
ChatMemory: 몇 개의 메시지를 저장할지, 어떤 기간 동안 저장할지
ChatMemoryRepository: 메시지 저장 및 조회

## ChatMemory
```java
public interface ChatMemory {
    void add(String conversationId, List<Message> messages); // 대화 기억 저장
    List<Message> get(String conversationId); // 대화 기억 검색
    void clear(String conversationId); // 대화 기억 삭제
}
```

ChatMemory의 구현체인 MessageWindowChatMemory를 활용해 메시지 윈도우 변경 가능
```java
ChatMemory memory = MessageWindowChatMemory.builder() 
    .maxMessages(10)
    .build();
```
## ChatMemoryRepository
실제로 메시지를 저장하고 꺼내오는 창고 역할

ChatMemoryRepository 인터페이스를 통해 저장소에 대화 기억을 저장한다.
- InMemoryChatMemoryRepository: 컴퓨터 하드웨어 메모리에 저장합니다.
- JdbcChatMemoryRepository: 관계형 데이터베이스를 저장합니다.
- CassandraChatMemoryRepository: ApacheCassandra를 이용해서 시계열로 저장합니다.