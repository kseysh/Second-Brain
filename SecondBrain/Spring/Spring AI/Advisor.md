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

## advisor 체인과 Chat Model 간 상호작용
![[Pasted image 20251024021352.png|300]]
- Spring AI 프레임워크는 사용자의 Prompt로부터 AdvisedRequest를 생성하고, 비어 있는 AdvisorContext 객체를 함께 생성합니다.
- 체인의 각 advisor는 해당 요청을 처리하며, 필요에 따라 요청을 수정할 수 있습니다. 또는 다음 엔티티 호출을 생략하여 요청을 차단할 수도 있습니다. 후자의 경우, 해당 advisor는 응답을 직접 작성할 책임이 있습니다.
- 프레임워크에서 제공하는 최종 advisor는 요청을 Chat Model에 전송합니다.
- Chat Model의 응답은 advisor 체인을 거쳐 다시 전달되며, AdvisedResponse로 변환됩니다. 이 응답에는 공유된 AdvisorContext 인스턴스가 포함됩니다.
- 각 advisor는 응답을 처리하거나 수정할 수 있습니다.
- 최종 AdvisedResponse는 ChatCompletion을 추출하여 클라이언트에 반환됩니다.

## Re-Reading Advisor

"[Re-Reading Improves Reasoning in Large Language Models](https://arxiv.org/pdf/2309.06275)"라는 논문에서 LLM의 추론 능력을 향상하는 Re-Reading (Re2) 기법

이 기법은 입력 프롬프트를 다음과 같이 보강합니다.

```java
{Input_Query}
Read the question again: {Input_Query}
```

Re2 기법을 적용하는 advisor 구현
```java
public class ReReadingAdvisor implements CallAroundAdvisor, StreamAroundAdvisor {

	private AdvisedRequest before(AdvisedRequest advisedRequest) { // 1️⃣

		Map<String, Object> advisedUserParams = new HashMap<>(advisedRequest.userParams());
		advisedUserParams.put("re2_input_query", advisedRequest.userText());

		return AdvisedRequest.from(advisedRequest)
			.userText("""
			    {re2_input_query}
			    Read the question again: {re2_input_query}
			    """)
			.userParams(advisedUserParams)
			.build();
	}

	@Override
	public AdvisedResponse aroundCall(AdvisedRequest advisedRequest, CallAroundAdvisorChain chain) { // 2️⃣
		return chain.nextAroundCall(this.before(advisedRequest));
	}

	@Override
	public Flux<AdvisedResponse> aroundStream(AdvisedRequest advisedRequest, StreamAroundAdvisorChain chain) { // 3️⃣ 
		return chain.nextAroundStream(this.before(advisedRequest));
	}

	@Override
	public int getOrder() { // 4️⃣
		return 0;
	}

	@Override
	public String getName() { // 5️⃣
		return this.getClass().getSimpleName();
	}
}
```
- before 메서드는 Re-Reading 기법을 적용하여 사용자의 입력 쿼리를 보강합니다.
- aroundCall 메서드는 비 스트리밍 요청을 가로채고 Re2 기법을 적용합니다.
- aroundStream 메서드는 스트리밍 요청을 가로채고 Re2 기법을 적용합니다.
- getOrder()를 통해 실행 순서를 제어할 수 있습니다. 값이 낮을수록 먼저 실행됩니다.
- getName()은 advisor의 고유 이름을 제공합니다.

