AI 모델이 실행 중에 호출할 수 있는 함수를 도구로 등록한다.

## 내부 동작 구조
Spring AI는 @Tool이 붙은 Bean 메서드를 스캔에 Tool Registry에 등록
모델 응답 중 function call 정보가 포함되어 있으면 Spring AI가 해당 Tool 이름을 매칭함

### name
도구를 식별할 때 사용
### description
모델이 도구를 언제, 어떻게 호출할지를 이해하는데 사용됨
지정하지 않으면, 메서드 이름이 설명으로 사용됨

## 반환값
반환 타입은 void도 허용되며, 직렬화가능해야 함