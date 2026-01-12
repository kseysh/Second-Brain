## 클래스 로더 계층 구조
![[Pasted image 20260112140612.png]]
### BootStrapClassLoader
JDK에 저장된 자바 핵심 라이브러리 로드 (ex) java.lang, java.util)
### ExtensionClassLoader
표준 핵심 라이브러리 외의 확장 라이브러리 로드 (ex) java.sql, java.xml, java.compiler)
### ApplicationClassLoader
사용자가 작성한 클래스 파일과 외부 라이브러리 로드

## 클래스 로딩 과정
![[Pasted image 20260112141610.png|200]]
### Loading 
자바 바이트 코드를 가져와서 JVM의 메모리에 로드
### Linking
바이트 코드를 가져와서
### Initialization
