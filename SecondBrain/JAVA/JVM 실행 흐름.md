1. HelloWorld.java 파일을 javac 컴파일러가 HelloWorld.class 파일인 바이트 코드로 변경
2. 클래스 로더가 필요할 때, 런타임 데이터 영역의 메서드 영역에 HelloWorld.class 적재
	1. Loading: .class 파일을 찾음. 바이트 코드 읽음, Method 영역에 저장 (HelloWorld.class 클래스 로딩)
	2. Linking: 런타임에서 쓸 수 있도록 정리 작업 
		1. Verfication: 바이트코드 검증
		2. Preparation: static 필드 메모리 할당
		3. Resolution: 심볼릭 레퍼런스를 실제 참조로 연결
	3. Initialization: static 초기화
3. 인터프리터가 바이트 코드를 한 줄씩 읽고, JVM 내부 기계어 루틴 실행
4. 반복이 임계점에 도달하면 greet()가 Hot Spot으로 판정되어 JIT가 Native Machine Code로 컴파일 진행 (ex) greet())