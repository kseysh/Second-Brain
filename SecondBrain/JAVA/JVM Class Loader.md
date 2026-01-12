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
바이트 코드를 가져와서 사용하기 위해 검증하는 과정
- Verifying
	- 읽어들인 클래스가 JVM 명세대로 구성되어 있는지 검사
- Preparing
	- 클래스가 필요로 하는 메모리 할당
- Resolving
	- 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경
### Initialization
static 필드를 설정된 값으로 초기화