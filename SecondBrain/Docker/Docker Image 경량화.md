## 기존 Docker Image
![[Pasted image 20260206112326.png]]

## Layer 별 상세 크기
![[Pasted image 20260206112444.png]]

## 경량화 기법
### 경량 이미지 사용
FROM amazoncorretto:17 -> FROM amazoncorretto:17-alpine
- JDK가 아닌 JRE만을 포함하고 있기 때문에 작다.
- Amazon Linux 2가 아닌, Alpine Linux로 패키지 관리를 apk로 하는 최소한의 유틸리티만 포함