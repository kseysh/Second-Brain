AI 모델이 실행 중에 호출할 수 있는 함수를 도구로 등록한다.

## 내부 동작 구조
Spring AI는 @Tool이 붙은 Bean 메서드를 스캔에 Tool Registry에 등록
모델 응답 중 function call 정보가 포함되어 있으면 Spring AI가 해당 Tool 이름을 매칭함