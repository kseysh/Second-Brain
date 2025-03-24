제가 진행하고 있는 프로젝트인 SOPT Makers에서는 여러 팀이 SOPT라는 동아리의 즐거운 활동을 위해서 여러 서비스를 제공해주는 단체에요.
SOPT Makers에는 공식 홈페이지 팀, Playground 팀, Crew 팀, Platform 팀, 앱 팀의 5가지 팀이 각자의 역할을 하며 개발을 해요.
하지만 이런 구성이다 보니 여러 EC2를 사용하는 상황이었고, AWS에서 IPv4의 유료화 등 점차 가격이 오름에 따라 EC2의 가격이 부담되는 상황이 발생했어요.

따라서 과거에는 각 팀마다 prod, dev EC2를 따로 두었지만, AWS 비용을 줄이기 위해서 dev는 한 개의 EC2로 통합하고, 각자의 환경은 docker를 통해 환경을 분리했습니다.
그 과정에서 dev 환경을 docker 환경으로 변경하고, 추가적으로 Docker를 이용해 무중단 배포를 진행할 수 있도록 변경하였습니다.




1. docker hub에서 토큰 저장하기
2. EC2에서 docker login & token 입력
3. docker pull 받기
4. docker container run --name {컨테이너 이름} -d -p 8086:8080 {이미지 이름}
5. nginx 이미지를 받아서 upstream blue green을 만들어 준다.
6. yml 파일 blue, green으로 나누고 port도 나누기


AWS dev key 변경하기


---
1. github trigger 발생
	1. github action 내에서 jar 파일 생성
	2. github action 내에서 docker build
	3. ECR REPO에 push
	4. ec2에 docker-compose.yml send
	5. ec2에 scripts 폴더 send
	6. ec2에서 ECR_REPO에서 docker pull 후 deploy.sh 실행
2. deploy.sh 실행
	1. 현재 실행 컨테이너 이름 가지고 오기
	2. deploy_container
		1. docker-compose pull
			1. ECR_REPO에서 docker pull 받는 명령
		2. docker-compose up
			2. 
	3. health_check
	4. stop_container
	5. reload_nginx
