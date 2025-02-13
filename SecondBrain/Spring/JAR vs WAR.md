# JAR (Java Archieve)
- JAVA 어플리케이션이 동작할 수 있도록 자바 프로젝트를 압축한 파일
- Class (JAVA리소스, 속성 파일), 라이브러리 파일을 포함함
- JRE(JAVA Runtime Environment)만 있어도 실행 가능함 
# WAR (Web Application archive)
- Servlet / Jsp 컨테이너에 배치할 수 있는 웹 애플리케이션(Web Application) 압축파일 포맷
- 웹 관련 자원을 포함함 (JSP, Servlet, JAR, Class, XML, HTML, Javascript)
- 사전 정의된 구조를 사용함 (WEB-INF, META-INF)
- 별도의 웹서버(WEB) or 웹 컨테이너(WAS) 필요
- 즉, JAR파일의 일종으로 웹 애플리케이션 전체를 패키징 하기 위한 JAR 파일이다.
# JAR vs WAR
리소스들을 하나의 파일로 패키징 하는 과정의 차이가 있는 것!
WAR 안에 JAR가 있는 것 처럼 WAR는 실행하기 위한 모든 파일을 묶고 있다.
![[Pasted image 20231012151407.png]]

꼭 WAR를 사용해야만 하는 이유(JSP를 사용하여 화면을 구성해야 한다 / 외장 WAS를 이용할 계획이 있다)가 아니라면 뭘 사용할지에 대한 완벽한 해답은 없다. 

Sprinag boot에서 가이드하는 표준은 JAR이므로 WAR를 사용해야하는 이유가 없다면 JAR를 사용하여 서비스하는 것이 좋은 선택이다.