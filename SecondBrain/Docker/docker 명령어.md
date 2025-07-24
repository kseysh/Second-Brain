`docker pull <image name>`

`docker images`

`docker run --name <container name> -d -p <host port>:<container port> <image name>`
-e: 환경 변수 설정
-d: (detach) 백그라운드 실행
-p: 포트 설정

`docker ps`
컨테이너 정보 확인
-a : 종료된 컨테이너 노출

`docker exec -it <container name> bash`

`docker stop <container name>`

`docker start <container name>`

`docker rm -f <container name>`
