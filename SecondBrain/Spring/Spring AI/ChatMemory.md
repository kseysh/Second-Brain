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

## Advisor
ChatMemory가 제공한 대화 기억을 실제 프롬프트에 포함시키는 작업을 해준다.
LLM에게 프롬프트를 전송하기 전에 전처리 작업으로 ChatMemory로부터 받은 대화 기억을 시스템 텍스트 또는 메시지 묶음으로 프롬프트에 추가한다.

![[Pasted image 20251024013910.png]]
### MessageChatMemoryAdvisor
ChatMemory에서 받은 대화 기억을 사용자 메시지와 AI 메시지들로 생성하고, 이 메시지들을 프롬프트에 추가한다.

MessageChatMemoryAdvisor를 ChatClient의 기본 Advisor로 추가하는 코드
```java
ChatClient chatClient = chatClientBuilder .defaultAdvisors(
    MessageChatMemoryAdvisor.builder(chatMemory).build() 
    )
.build();
```
### PromptChatMemoryAdvisor
ChatMemory로부터 받은 대화 기억을 텍스트 형태로 시스템 메시지에 포함

PromptChatMemoryAdvisor를 ChatClient의 기본 Advisor로 추가하는 코드
```java
ChatClient chatClient = chatClientBuilder 
    .defaultAdvisors(
        PromptChatMemoryAdvisor.builder(chatMemory).build() 
    )
    .build();
```