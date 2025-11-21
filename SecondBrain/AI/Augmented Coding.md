코드 품질, 복잡도, 테스트 커버리지 등 개발자가 주도권을 가지고 AI를 활용하는 방법

## 하는 방법
Red -> Green -> Refactor를 활용한다
- (Red) 실패하는 테스트를 만든다.
- (Green) 테스트를 통과하기 위한 최소 코드를 만든다.
- (Refactor) 코드를 정리한다.
이 과정을 통해 AI가 작성해야 하는 코드를 제대로 정의하고, 코드의 양을 최소한으로 억제한다.

1. 구현하고자 하는 기능의 PRD(Product Requirements Document)를 생성해달라고 요청한다
2. claude.md를 활용한다. ([Github](https://github.com/KentBeck/BPlusTree3/blob/ca80e4d85a99cd0af2effe717f709d43e80403bc/rust/docs/CLAUDE.md))
3. claude.md를 활용해 Claude custom commands를 만든다.
4. PRD를 통해 plan.md를 만든다.
5. plan.md에 따라 tdd를 진행한다/.