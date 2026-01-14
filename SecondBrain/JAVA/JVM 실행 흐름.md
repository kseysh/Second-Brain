1. HelloWorld.java 파일을 javac 컴파일러가 HelloWorld.class 파일인 바이트 코드로 변경
2. 클래스 로더가 필요할 때, 런타임 데이터 영역의 메서드 영역에 바이트 코드 적재 (런타임 데이터 영역 자체는 불변이지만, 이 안에 어떤 바이트 코드를 올릴지는 동적으로 결정됨)
3. 인터프리터가 바이트 코드를 한 줄씩 읽고, JVM 내부 기계어 루틴 실행
4. 반복되는 코드인 Hot Spot은 JIT가 Native Machine Code로 컴파일 진행