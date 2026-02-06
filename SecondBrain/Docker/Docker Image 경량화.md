## 기존 Docker Image
![[Pasted image 20260206112326.png]]

## Layer 별 상세 크기
![[Pasted image 20260206112444.png]]

## 경량화 기법
### 경량 이미지 사용
FROM amazoncorretto:17 -> FROM amazoncorretto:17-alpine
- JDK가 아닌 JRE만을 포함하고 있기 때문에 작다. (프로덕션에서는 컴파일된 jar만 실행하므로)
- Amazon Linux 2가 아닌, Alpine Linux로 패키지 관리를 apk로 하는 최소한의 유틸리티만 포함한다.

### 멀티스테이지 빌드
Intellij를 하나의 컨테이너라고 보면, Java를 실행하는데 필요한 파일은 .jar 파일 하나뿐이지만, 불필요하게 .java 및 .class 파일이 포함되어 있다고 생각할 수 있다.
따라서 우리는 빌드한 결과물인 .jar 파일 하나만을 Docker Container로 옮기는 방식을 멀티 스테이지 빌드라고 한다.

멀티 스테이지 빌드는 아래의 단계를 지난다.
1. build 도구가 설치된 기반 Image : 로컬 환경에서 소스 코드를 복사하여 build 명령 실행
2. Unit Test Framework가 설치된 기반 Image : Build한 바이너리를 복사하여 단위 테스트 수행
3. Runtime이 설치된 기반 Image : 테스트가 통과한 바이너리들을 복사하여 애플리케이션 실행 
- 애플리케이션 이식성 확보 → 클라우드 상에 배포 가능
- Docker만 있다면 Container를 통해 어떤 환경에서도 애플리케이션 빌드 및 실행 가능
