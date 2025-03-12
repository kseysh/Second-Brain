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
			1. 
	3. health_check
	4. stop_container
	5. reload_nginx
