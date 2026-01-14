런타임 데이터 영역에 배치된 바이트코드를 명령어 단위로 읽어서 실행

### Interpreter
바이트 코드 명령어를 하나씩 읽어서 해석하고 실행
인터프리터는 바이트코드를 해석하고, JVM 내부에 이미 기계어로 컴파일된 구현을 실행한다

#### Interpreter 과정
Fetch → Decode → Execute  
- Fetch(PC가 가리키는 Opcode를 읽음) 
- Decode(Opcode의 피연산자 가져옴) 
- Execute(Opcode의 실제 구현을 실행)

### JIT 컴파일러
반복되는 코드(hot spot)을 발견하여 바이트 코드 전체를 컴파일하여 Native Machine Code(≒ 기계어)로 변경하고 이후에 메서드를 캐싱해두었다가 네이티브 코드로 직접 실행하는 방식

### GC
Heap 영역에서 사용하지 않는 메모리 회수