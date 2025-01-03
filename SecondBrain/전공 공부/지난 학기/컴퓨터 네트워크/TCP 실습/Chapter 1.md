# 네트워크 프로그래밍이란?
- 소켓이라는 것을 기반으로 프로그래밍을 하기 때문에 소켓 프로그래밍이라고도 한다.
- 네트워크로 연결된 둘 이상의 컴퓨터 사이에서의 데이터 송수신 프로그램의 작성을 의미한다.
## 소켓에 대한 간단한 이해
- 네트워크의 연결 도구
- 운영체제에 의해 제공이 되는 소프트웨어적인 장치
- 소켓은 프로그래머에게 데이터 송수신에 대한 물리적, 소프트웨어적 세세한 내용을 신경 쓰지 않게 한다.
## 연결요청을 허용하는 소켓의 생성과정
1. 1단계. 소켓의 생성 socket 함수호출
2. 2단계. IP와 PORT번호의 할당 bind 함수호출
3. 3단계. 연결요청 가능상태로 변경 listen 함수호출
4. 4단계. 연결요청에 대한 수락 accept 함수호출
### 1. 소켓의 비유와 분류
- TCP 소켓은 전화기에 비유될 수 있다.
- 소켓은 `socket` 함수의 호출을 통해서 생성한다.
- 단, 전화를 거는 용도의 소켓과 전화를 수신하는 용도의 소켓 생성 방법에는 차이가 있다.
### 2. 소켓의 주소 할당 및 연결
- 전화기에 전화번호가 부여되듯이 소켓에도 주소정보가 할당된다.
- 소켓의 주소정보는 IP와 port번호로 구성이 된다.
### 3. 연결요청이 가능한 상태의 소켓
- ex) 걸려오는 전화를 받을 수 있는 상태
- 전화를 거는 용도의 소켓은 연결요청이 가능한 상태의 소켓이 될 필요가 없다.
- 이는 걸려오는 전화를 받는 용도의 소켓에서 필요한 상태이다.
### 4. 연결요청의 수락
- ex) 걸려오는 전화에 대해서 수락의 의미로 수화기를 드는 것
- 연결요청이 수락되어야 데이터의 송수신이 가능하다
- 수락된 이후에 데이터의 송수신은 양방향으로 가능하다.
## 서버 소켓, 리스닝 소켓
연결요청을 허용하는 프로그램을 가리켜 일반적으로 서버(Server)라 한다.
- 서버는 연결을 요청하는 클라이언트보다 먼저 실행되어야 한다.
- 클라이언트보다 복잡한 실행의 과정을 거친다.
- 이렇게 생성된 소켓을 가리켜 서버 소켓 또는 리스닝 소켓이라 한다.

## 실습
실습 코드만 이해하면 될 듯.
### hello_client.c
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>

void error_handling(char *message);

int main(int argc, char* argv[])
{
	int sock; // 파일 디스크립터 값 
	struct sockaddr_in serv_addr;
	char message[30];
	
	if(argc!=3){
		printf("Usage : %s <IP> <port>\n", argv[0]);// 서버의 IP번호와 서버의 port 번호
		exit(1);
	}
	
	sock=socket(PF_INET, SOCK_STREAM, 0); // 소켓 생성, file descripter 반환
	if(sock == -1)
		error_handling("socket() error");
	// 클라이언트가 사용할 IP번호와 port 번호를 할당해야 하는데 할당하지 않은 이유는
	// 클라이언트는 자동으로 할당되기 때문이다.
	// 시스템이 port 번호를 자동으로 할당하고, 내가 쓰는 IP번호는 알고 있기 때문에 따로 bind 해주지 않아도 된다.
	memset(&serv_addr, 0, sizeof(serv_addr));
	serv_addr.sin_family=AF_INET;
	serv_addr.sin_addr.s_addr=inet_addr(argv[1]); // 서버의 IP 주소가 들어감
	serv_addr.sin_port=htons(atoi(argv[2])); // 서버의 port 번호가 들어감
		
	if(connect(sock, (struct sockaddr*)&serv_addr, sizeof(serv_addr))==-1)
	// socket에 connect를 이용해 연결요청을 한다. 
	// 상대방의 서버 정보는 serv_addr에 저장된다.
		error_handling("connect() error!");
	// 연결이 되면 서버에서 데이터를 보내준다.
	// 서버에서 보낸 데이터는 socket에서 read로 읽어 message에 저장하고, size는 str_len에 저장한다.
	if(read(sock, message, sizeof(message)-1) == -1)
		error_handling("read() error!");
	
	printf("Message from server: %s \n", message);  
	close(sock);
	return 0;
}

void error_handling(char *message)
{
	fputs(message, stderr);
	fputc('\n', stderr);
	exit(1);
}

```

### hello_server.c
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>

void error_handling(char *message);

int main(int argc, char *argv[])
{
	int serv_sock;
	int clnt_sock;

	struct sockaddr_in serv_addr;
	struct sockaddr_in clnt_addr;
	socklen_t clnt_addr_size;

	char message[]="Hello World!";
	
	if(argc!=2){
		printf("Usage : %s <port>\n", argv[0]);
		exit(1);
	}
	
	serv_sock=socket(PF_INET, SOCK_STREAM, 0); // 소켓을 생성한다.
	if(serv_sock == -1)
		error_handling("socket() error");
	
	memset(&serv_addr, 0, sizeof(serv_addr));
	serv_addr.sin_family=AF_INET;
	serv_addr.sin_addr.s_addr=htonl(INADDR_ANY); // 서버가 쓸 IP 주소 serv_addr 구조체에 저장
	serv_addr.sin_port=htons(atoi(argv[1])); // 서버가 쓸 port 주소 serv_addr 구조체에 저장
	
	if(bind(serv_sock, (struct sockaddr*) &serv_addr, sizeof(serv_addr))==-1)
	// 소켓에 서버가 사용할 주소를 bind를 이용해 할당한다
		error_handling("bind() error"); 
	
	if(listen(serv_sock, 5)==-1) // serv_sock가 listen 함수를 이용해 전화받을 수 있는 상태로 바뀐다. (일반 소켓이 서버 소켓으로 바뀐다.)
		error_handling("listen() error");
	
	clnt_addr_size=sizeof(clnt_addr);  
	clnt_sock=accept(serv_sock, (struct sockaddr*)&clnt_addr,&clnt_addr_size); // 전화를 받는 함수, 상대방에 대한 정보는 clnt_addr에 저장하고 clnt_sock에 저장한다.
	if(clnt_sock==-1)
		error_handling("accept() error");  
	
	write(clnt_sock, message, sizeof(message));
	close(clnt_sock);	
	close(serv_sock);
	return 0;
}

void error_handling(char *message)
{
	fputs(message, stderr);
	fputc('\n', stderr);
	exit(1);
}

```