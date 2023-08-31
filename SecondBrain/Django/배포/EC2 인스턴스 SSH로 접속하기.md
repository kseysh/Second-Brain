# EC2란?
Amazon Elastic Compute Cloud로 안전하고 크기 조정이 가능한 컴퓨팅 용량을 클라우드에서 제공하는 웹 서비스이다.

# SSH란?
Secure Shell Protocol로 네트워크 프로토콜 중 하나로 클라이언트와 서버가 인터넷과 같은 Network를 통해 통신을 할 때, 보안적으로 안전하게 통신을 하기 위해 사용하는 프로토콜이다. 보통 Password 인증과 RSA 공개키 암호화 방식으로 연결한다.

## EC2를 생성하고 SSH로 접속이 되지 않을 때 확인해야할 사항

1. port번호를 확인한다.
	1. Listen Port를 지정하여 서버에서도 port 변경을 할 수 있고, 접속 명시를 통해 클라이언트에서도 지정할 때 변경이 가능하다.
2. public ip를 사용했는지를 확인한다.
	1. 54.5.x.x , 2.x.x.x, 34.6.x.x 같은 public ip인지 확인한다.
	2. 10.0.x.x, 192.168.x.x, 172.16~32.x.x 같은 private ip이면 접속이 되지 않을 수 있다.
3. Security Group Rule을 확인한다.
	1. aws는 기본 방화벽 솔루션으로 시큐리티그룹을 확인한다.
	2. 내 클라이언트의 IP와 시큐리티에 허용되어져 있는 IP가 올바르게 허용이 되어 있어야 접속이 가능하다.
4. EC2 인스턴스가 퍼블릭 서브넷에 위치에 있는지 확인한다. (private subnet에 위치해 있다면 외부와 통신을 할 수 없기 때문이다.)
	1. 해당 서브넷이 연결된 라우팅 테이블을 확인하고 Routes에 0.0.0.0/0 igw-xxxxx인를 확인한다.

## 1. EC2 인스턴스 생성
컴퓨터에서 window os를 사용하고 AMI는 Ubuntu/t2.micro(프리 티어)를 사용한다는 가정하에 설명하겠습니다!

키 페어가 없다면 키페어를 생성해주어야 합니다.
보통 RSA 방식을 많이 사용하므로 RSA방식과, window이므로 .ppk를 선택해줍니다.

그리고 방화벽 부분에서 SSH 트래픽 허용 / HTTPS 트래픽 허용 / HTTP 트래픽 허용을 용도에 맞게 선택해줍니다.
저는 모두 선택하였고 위치 무관 0.0.0.0/0으로 선택해주었습니다.
이렇게 선택하고 인스턴스 시작을 눌러주시면 됩니다.

window라면 key pair를 위해서 putty라는 것을 설치해주어야 합니다. 만약 실수로 window를 사용하는데 .pem를 설치하였다면 puttygen을 이용해  .ppk로 private key를 생성해주실 수 있습니다.

이후 putty 왼쪽 카테고리 부분에서 Auth ->  Credentials에서 Private key file for authentication에서 ppk의 위치를 Browse해서 넣어주고, 다시 왼쪽 카테고리 부분에서 Session을 누르고  Host Name에 USERNAME@instance_public_ip 이런식으로 입력하고 Open을 눌러주면 됩니다. 
우분투는 기본 username이 ubuntu이고, 인스턴스의 public ip는 생성했었던 인스턴스 정보에 들어가게 되면 퍼블릭 IPv4 주소가 public ip라고 할 수 있습니다. 따라서 저는 Host Name에 ubuntu@15.164.165.xxx 이런 방식으로 적어주게 되었습니다. (퍼블릭 IPv4 DNS 링크를 붙여넣어도 됩니다.)

Accept를 눌러주면 서버가 실행됩니다.(퍼블릭 IPv4 DNS 링크를 넣었다면 login as: ubuntu로 작성해주면 됩니다.)
