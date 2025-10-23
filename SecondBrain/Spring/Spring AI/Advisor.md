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