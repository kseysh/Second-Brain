
![[Pasted image 20260112141254.png|300]]

1. JVM이 OS로부터 메모리 할당
2. javac(자바 컴파일러)가 .java 파일(자바 소스코드)을 .class 파일(자바 바이트 코드)로 컴파일
3. Class Loader는 **동적 로딩**을 통해 필요한 클래스들을 로딩 및 링크하여 Runtime Data Area에 올림
4. Runtime Data Area에 로딩된 바이트 코드는 Execution Engine을 통해 해석