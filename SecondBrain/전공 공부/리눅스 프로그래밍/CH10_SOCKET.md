### 제10장 소켓

#### 인터넷 모델 (TCP/IP 프로토콜 스위트) (1/2)
![[Pasted image 20241125130308.png|400]]
![[Pasted image 20241125130318.png|400]]
![[Pasted image 20241125130338.png|400]]
## 소켓
- 소켓은 통신 엔드포인트의 추상화입니다.
- 표준 유닉스 파일 디스크립터를 사용하여 다른 프로그램과 통신하는 방식입니다
*"유닉스에서는 모든 것이 파일이다."*
- 파일 디스크립터는 단순히 열린 파일과 연결된 정수 값입니다. 이 파일은 네트워크 연결, FIFO, 파이프, 터미널, 디스크 파일 또는 기타 다양한 항목이 될 수 있습니다.
- 인터넷을 통해 다른 프로그램과 통신하려면 파일 디스크립터를 통해야 합니다.
## 두 가지 인터넷 소켓 유형
- 데이터그램 소켓 (UDP 사용):
  - 연결 없는 프로토콜
  - 흐름 제어 및 오류 제어 없음 (신뢰성 낮음)
  - 작은 메시지 전송에 사용
  - 멀티캐스트 및 브로드캐스트 지원
- 스트림 소켓 (TCP 사용):
  - 연결 지향 프로토콜
  - 흐름 제어 및 오류 제어 메커니즘 사용 (신뢰성 높음)

![[Pasted image 20241125130956.png|300]]
recv와 send는 read write와 마지막 매개변수이외에는 다 같다. 마지막 매개변수가 필요없다면 read, write로도 충분하다.
상대방이 close했는데 send하면 SIGPIPE가 날아온다. pipe했을 때와 같다.
![[Pasted image 20241125133311.png|500]]
서버 소켓 생성: 
SOCK_STREAM -> TCP를 사용하겠다.
SOCK_DGRAM -> UDP를 사용하겠다.
sockaddr: socket은 인터넷만 사용하는 것이 아니므로 generic address로 setting한다.
이 중 인터넷을 사용하는 sockaddr이 sockaddr_in이고, bind는 sockaddr을 사용해야 하므로 sockaddr로 type casting을 해준다.

![[Pasted image 20241125133324.png|500]]
어떤 소켓을 쓰는지에 대해 문제 나올듯 어떤 소켓 쓰는지만 확인해두기
# 서버와 클라이언트 소켓 구현의 이해 UDP
![[Pasted image 20241125131304.png|400]]
# Addressing
## Internet addressing
- IP addresses는 domain names라고 불리는 식별자의 집합이다.
	- 165.246.10.2 is mapped to inha.ac.kr
![[Pasted image 20241202122723.png|500]]
aton과 addr 구분하기
## Main Structure
![[Pasted image 20241125131522.png|400]]
generic address -> 여러가지 네트워크를 만들기 위해 필요한 추상적 객체
## Byte Ordering
![[Pasted image 20241125131537.png|400]]
## Confusing the byte ordering
![[Pasted image 20241125131555.png|400]]
## Network byte order
![[Pasted image 20241125131626.png|400]]
TCP/IP는 항상 빅 엔디언을 사용한다.
- 그래서 애플리케이션은 때때로 프로세서의 바이트 순서와 네트워크 바이트 순서 사이를 변환해야 합니다. (hton)
# Socket interface
## socket(2) system call
![[Pasted image 20241125131727.png|400]]
인터넷에서는 domain과 type만 지정하면 된다. AF_INET, SOCK_STREAM
## Selecting the protocol
### Connection-Oriented vs Connectionless
• Connection oriented (streams)
– `sd = socket(AF_INET, SOCK_STREAM, 0);`
• Connectionless (datagrams):
– `sd = socket(AF_INET, SOCK_DGRAM, 0);`
인터넷(AF_INET)에서는 이것이 각각 TCP와 UDP에 해당합니다.
## bind(2) system call
```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t len);
```

```c
struct sockaddr {
	u_short sa_family; char sa_data[14]; /* address family */
	/* variable-length address */

struct in_addr {
	u_long s_addr; };
};

/* IPv4 address */

struct sockaddr_in {
	u_short sin_family; /* address family */
	u_short sin_port; /* port number */
	struct in_addr sin_addr; /* IPv4 address , INADDR_ANY */
	char sin_zero[8];
};
```
## listen(2) system call
`int listen(int sockfd, int backlog);`
backlog => 대기 Queue의 size
![[Pasted image 20241128093813.png|400]]
## `accept(2)` 시스템 호출
```c
int accept(int sockfd, struct sockaddr *restrict addr, socklen_t *restrict len);
```
- 서버가 클라이언트의 연결 요청을 받으면, 새로운 소켓을 생성하여 특정 통신을 처리해야 합니다.
  - 첫 번째 소켓: 연결 수립 전용
  - 두 번째 소켓: 특정 통신을 위한 소켓
- 매개변수:
  - `addr`: 연결 지향 통신이므로 NULL로 넣어도 된다.
  - `len`: NULL
![[Pasted image 20241128093906.png|400]]
## `connect` system call
```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t len);// addr: 서버 주소
```
![[Pasted image 20241128094033.png|400]]
## 데이터 보내고 받기
```c
ssize_t recv(int sockfd, void *buf, size_t nbytes, int flags);
// 반환: 메시지의 길이(바이트 단위), 사용 가능한 메시지가 없고 상대방이 정상적으로 종료한 경우 0, 오류 발생 시 -1

ssize_t send(int sockfd, const void *buf, size_t nbytes, int flags);
// 반환: 전송된 바이트 수(정상인 경우), 오류 발생 시 -1
```
flags == 0일 때: read(), write()와 동일
## 연결 종료
- 소켓의 반대쪽 프로세스가 예기치 않게 종료될 경우, 적절히 처리하는 것이 매우 중요합니다.
- 프로세스가 연결이 끊어진 소켓에 데이터를 쓰거나 보낼 경우 **SIGPIPE 신호**를 받습니다.
- `read` 또는 `recv`가 **0을 반환**하면 파일의 끝을 나타내며, 따라서 연결 종료를 의미합니다.
### server process example
![[Pasted image 20241128094405.png|500]]
![[Pasted image 20241128094418.png|500]]
### client process example
![[Pasted image 20241128094459.png|500]]
![[Pasted image 20241128094510.png|500]]
## 메시지 송수신
## Sending and receiving messages
```c
ssize_t recvfrom(int sockfd, void *restrict buf, size_t len, int flags, struct sockaddr *restrict send_addr, socklen_t *restrict addrlen);
// Returns: length of message in bytes, 0 if no messages are available and peer has done an orderly shutdown, or -1 on error
ssize_t sendto(int sockfd, const void *buf, size_t nbytes, int flags, const struct sockaddr *dest_addr, socklen_t destlen);

```
- 매개변수:
  - `send_addr`: 메시지를 보낸 장치의 **주소 정보**
  - `dest_addr`: 메시지를 보낼 **피어의 주소**
### server process example
![[Pasted image 20241128094642.png|500]]
### client process example
![[Pasted image 20241128094702.png|500]]