다음 세 가지 애노테이션의 조합
1. @Configuration: 이 클래스가 Spring IoC 컨테이너에 의해 Bean 정의의 소스로 사용될 수 있음을 나타냅니다.
2. @EnableAutoConfiguration: build.gradle, yml의 속성 설정 및 @Configuration을 기반으로 Spring Boot가 자동으로 필요한 Bean을 추가하는 방식
3. @ComponentScan: 웹 컨트롤러 클래스 및 기타 구성 요소를 자동으로 검색하고 Spring 애플리케이션 컨텍스트에서 Bean으로 등록할 수 있도록 구성 요소 스캐닝을 활성화합니다.