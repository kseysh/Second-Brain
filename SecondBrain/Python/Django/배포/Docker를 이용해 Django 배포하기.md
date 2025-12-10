window에 기반하여 설명합니다.
블로그에 올리기 전 수정해주기기
# 1. 어플리케이션 컨테이너화

Docker 다운로드 및 설치 : https://docs.docker.com/get-docker/

manage.py가 있는 폴더에 `Dockerfile`이라는 이름의 빈 파일을 생성한다. 필요하다면 `.dockerignore` 파일도 생성한다.

사용하고 있던 파이썬 버전을 확인해주기 위해 가상환경 내에서 터미널을 이용해 
`python --version`을 입력하여 파이썬 버전을 확인해준다.

```
(myenv) C:\Users\USER\Desktop\Django-Blog-Template-main>python --version
Python 3.10.10
```

그 후 `Dockerfile`을 아래와 같이 작성해준다.
```
FROM python:3.10.10
# python 3.10.10 버전이라는 베이스 이미지를 사용하여 새로운 이미지를 생성한다.

WORKDIR /Django-Blog-Template-main
# 이 문장은 작업 디렉토리를 '/Django-Blog-Template-main'으로 설정한다. 이 디렉토리는 이미지 내에서 모든 작업이 수행될 위치이다. 쉽게는 manage.py를 가지고 있는 폴더의 위치라고 생각하면 된다.
# (저는 manage.py의 위치가 '/Django-Blog-Template-main/manage.py'이므로 WORKDIR을 이렇게 설정한 것 입니다.) 보통 우분투에서 작업 디렉토리는 /usr/src/app에 저장되므로 이렇게 설정해도 무관하다.

COPY . .
# 이 문장은 현재 디렉토리의 모든 파일과 폴더를 현재 작업 디렉토리(`/Django-Blog-Template-main`)로 복사합니다. 즉, 현재 디렉토리의 모든 파일과 폴더가 이미지 내의 `/Django-Blog-Template-main` 디렉토리로 복사됩니다.

RUN pip install -r requirements.txt
# 이 문장은 Django를 이미지내에서 실행시키기 위해 pip으로 설치해야할 내용들을 설치하는 명령을 실행합니다. 
# requirements.txt에 적혀있는 pip들을 설치하게 됩니다.

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
# 이 문장은 컨테이너가 실행될 때 기본으로 실행할 명령을 지정합니다. WORKDIR에서 지정한 디렉터리에서 실행되는 문장입니다. 만약 gunicorn을 사용한다면 CMD ["gunicorn", "--bind", "0.0.0.0:8000", "config.wsgi:application"] 이 CMD를 사용합니다.

EXPOSE 8000
# django 서버의 포트를 8000으로 지정하였으므로 Docker의 컨테이너 또한 8000 포트를 열어줍니다.
# 참고로 장고의 기본 포트번호는 8000입니다.
```

> 이미지란?
> 도커 이미지는 소스 코드, 라이브러리, 종속성, 도구 및 응용 프로그램을 실행하는데 필요한 기타 파일을 포함하는 불변(변경 불가) 파일이다.

>requirements.txt에 관해서
>requirements.txt는 pip으로 설치할 목록이 적혀있는 text파일로 pip을 설치하였던 가상환경 내에서 `pip freeze > requirements.txt` 명령어를 통해 생성할 수 있다.


`Dockerfile`이 있는 디렉토리(보통 최상위 폴더에 놓는다)로 이동한 다음 해당 코드를 통해서 image를 빌드해준다. ( 뒤에 .이 있는 것에 유의하자 )

`docker build -t test-image .`

`-t` 플래그는 이미지에 태그를 지정한다 (`--tag`로 쓸 수도 있다.) 이미지를 사람이 읽을 수 있는 이름으로 지정하면 된다. 이미지 이름을 지정했으므로 컨테이너를 실행할 때 해당 이미지를 참조할 수 있게 된다.
여기서 `docker build` 끝에 있는 .은 Docker에 현재 directory에서 Dockerfile을 찾아야함을 알려준다.

docker hub를 이용하려면 도커허브 닉네임과 슬래시 뒤에 도커 이미지 태그을 적어준다.
ex) `docker build -t kseysh/stalk-image .` (멋사에서 배포하면서 수 없이 쳤던 그 코드...)

`docker run`을 사용하여 컨테이너를 시작하고 방금 만든 이미지의 이름을 지정한다.
`docker run -d -p 8000:8000 kseysh/test-image`
여기서 플래그 `-d`(`--detach`라고 쓰기도 한다.)는 백그라운드에서 컨테이너를 실행한다. 플래그 `-p`(`--publish`라고 쓰기도 한다.)는 호스트와 컨테이너 간의 포트 매핑을 생성한다.
`8000:8000` 은 `HOST:CONTAINER`의 문자열 형식을 가진다.

이후 `http://localhost:8000` 으로 이동이 된다면 Docker 이미지를 생성하는 것에 성공한 것이다.

만약 소스코드를 업데이트하고 싶다면 Docker Desktop에서 만들어진 container의 오른쪽에 Actions의 휴지통 버튼을 눌러 컨테이너를 삭제하고, 컨테이너를 생성했을 때와 동일하게 
`docker build -t test-image .`으로 업데이트된 버전을 빌드하고
`docker run -d -p 8000:8000 kseysh/test-image` 을 사용하여 새 컨테이너를 시작한다.
![[Pasted image 20230824151536.png]]

이렇게 컨테이너를 생성하면 docker는 컨테이너의 이름을 아무렇게나 설정하는데, 컨테이너의 이름을 바꾸고 싶다면 cmd창에서 `docker rename oldname newname`의 명령어로 이름을 변경해줄 수 있다.

그 후 docker desktop에서 container에 들어가 `push to hub` 버튼을 누르거나, cmd에서 `docker push kseysh/test-image`명령어를 통해 docker hub에 image를 push 해준다. 이후  https://labs.play-with-docker.com/?_gl=1*u7msw9*_ga*MTUzMDQwODQyNy4xNjg3Nzc3Mzg1*_ga_XJWPQMJYHQ*MTY4ODQ0NDE3MC4zLjEuMTY4ODQ1NjA5Mi42MC4wLjA.
에서 add new instance를 하고 입력창이 뜨면 `docker run -dp 0.0.0.0:8000:8000 kseysh/test-image`를 입력하고 8000번 포트로 접속해보며 0.0.0.0 호스트의 모든 인터페이스에서 컨테이너의 포트가 노출되어 외부에서 사용할 수 있는지를 확인할 수 있다.


# 2. AWS ec2와 Docker 연동
ec2 인스턴스에서 연결을 눌러 ubuntu 서버로 이동한다.
이 환경은 초기에 텅 비어있는 것이나 다름없기 때문에 설치 가능한 패키지 리스트를 `sudo apt-get update`를 통해 설치한다.
아래 코드를 입력하여 필요한 패키지를 설치해준다.
```
sudo apt-get install \  
apt-transport-https \  
ca-certificates \  
curl \  
gnubg \  
lsb-release
```
이후 curl 명령어를 통해 Docker의 GPG key를 불러와서 apt에 추가한다. 이 과정을 통해서 apt로 docker 패키지를 설치할 수 있도록한다.
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

stable repository로 set up하기 위해 아래 코드를 작성해준다.
`sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

공식문서에도 우분투에서 docker engine을 설치하는 방법이 나와있다!
=> https://docs.docker.com/engine/install/ubuntu/

`sudo apt-get update` 이후
`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

Docker에서 제공하는 hello-world라는 Image를 받아서 Docker가 잘 작동하는지 확인할 수 있다.
`sudo docker run hello-world`를 작성하면 "Hello from Docker!" 라는 문장이 나오면 Docker가 잘 작동하고 있다고 할 수 있다.

`sudo docker build -t test-image .`을 작성하여 Container Image를 생성한다.

`sudo docker run -d -p 8000:8000 test-image`를 작성하면 Container를 실행할 수 있다.

`curl localhost:8000` 이 명령어로 localhost에 접속하여 정상적인 html이 출력된다면, 성공이다.

이 Container가 EC2에 잘 배포되었는지 확인하려면  인스턴스의 public IP 주소와 포트번호로 접속해보면 확인할 수 있다.

도커 이미지와 참조하는 컨테이너를 모두 삭제하려면 `docker rmi -f 이미지이름(혹은 이미지id)`로 삭제해준다.

> 저는 여기서 오류가 나서 조금 헤메였었는데요, 찾아보면서 보안 그룹 설정에서 8000번 포트를 인바운드 규칙으로 허용해주었고 성공하였습니다.
> 혹시라도 이 방법으로도 되지 않으신다면,
> 네트워크 ACL 설정 : EC2 인스턴스의 네트워크 ACL에서도 포트 8000을 허용하고 있는지 확인하고,
> 방화벽 또는 기타 네트워크 장치 설정 : EC2 인스턴스를 호스팅하는 네트워크에서 방화벽이나 기타 네트워크 장치를 사용하는 경우 해당 장치에서도 포트 8000을 허용해야한다고 합니다.

나중에 개발을 하면서 도커 이미지가 계속 변경되는 상황이 발생하게 될 텐데 그 때는 
docker hub에서 로컬에서의 도커 이미지를 삭제해주고, 다시 vscode로 돌아가서 `sudo docker build -t kseysh/stalk-image .` 를 통해 새로운 도커 이미지를 생성하고 docker hub에서 push to hub 버튼을 통해 도커 허브에 새로운 도커 이미지를 올려둔다. 
이후 우분투에서 원래 있던 도커 이미지를 새로운 도커이미지로 변경해주어야 하므로 아래 명령어를 통해 변경해준다.
`sudo docker pull 변경하려는도커이미지이름`
이후 원래 실행되고 있던 도커 컨테이너들을 아래 명령어를 통해 중지 및 삭제한다. 
`sudo docker stop 중지하려는도커컨테이너이름`
`sudo docker rm 중지하려는도커컨테이너이름`
그리고 새롭게 도커 허브에 올라온 도커 이미지를 우분투에 컨테이너로서 올려둔다.
`sudo docker run -d -p 8000:8000 kseysh/stalk-image`

