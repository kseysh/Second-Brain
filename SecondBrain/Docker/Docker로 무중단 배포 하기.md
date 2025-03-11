1. docker hub에서 토큰 저장하기
2. EC2에서 docker login & token 입력
3. docker pull 받기
4. docker container run --name {컨테이너 이름} -d -p 8086:8080 {이미지 이름}
5. nginx 이미지를 받아서 upstream blue green을 만들어 준다.
6. yml 파일 blue, green으로 나누고 port도 나누기


```shell
server {
    server_name app.dev.sopt.org;
    include /etc/nginx/conf.d/app-url.inc; 

    location / {
        proxy_pass $service_url;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

} 

server {
    listen 80;
    server_name app.dev.sopt.org;
}
```