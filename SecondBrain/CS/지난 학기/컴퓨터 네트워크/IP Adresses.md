![[Pasted image 20241105232543.png|500]]

![[Pasted image 20241105232637.png|500]]
시작 주소가 0으로 시작하면 Class A => 0 - 127로 시작 => 20억
시작 주소가 10으로 시작하면 Class B => 128 - 191로 시작 => 10억
시작 주소가 110으로 시작하면 Class C => 192 - 223로 시작 => 5억
시작 주소가 1110으로 시작하면 Class D => 224 - 239로 시작 => 2.6억
시작 주소가 1111으로 시작하면 Class E =>  240 - 255로 시작 =>2.6억

![[Pasted image 20241105233212.png|500]]
![[Pasted image 20241105234412.png|500]]
클래스마다 끊어읽는 곳이 다르다.
 NetId: 각 네트워크마다 대표하는 번호가 있는데 그것을 Network Id 즉 NetId라고 한다.
 HostId: 네트워크 아이디 내에서 가질 수 있는 번호
## Class
![[Pasted image 20241105235029.png|400]]
Class A는 첫 번째 바이트만 NetId, 나머지는 HostId이다.
따라서 class A는 128개의 block이 있고 한 개의 블록은 2의 24승인 1600만개의 HostId를 표현할 수 있다.
class를 구매하면 hostId는 마음대로 부여할 수 있는 권한을 얻게 된다.
![[Pasted image 20241106153006.png|500]]
![[Pasted image 20241106153016.png|500]]
실제로 사용되는 것은 보통 ABC이다.

![[Pasted image 20241106153030.png|500]]
multicast용도로 사용
![[Pasted image 20241106153037.png|500]]
나중을 위해 잡아둔 것

IP는 이렇게 두 가지로 끊어읽을 수 있다.
![[Pasted image 20241106153139.png|500]]

![[Pasted image 20241106153224.png|500]]
![[Pasted image 20241106153440.png|500]]
시작 주소는 대표 주소로, 마지막 주소는 directed broadcast address 용도로 사용된다.

![[Pasted image 20241106153642.png|500]]
일반적으로 라우터가 IP를 가지고 있고 Switch는 IP를 가지지 않는다.
Switch는 MAC주소를 사용한다.

![[Pasted image 20241106154024.png|600]]
박스 = 하나의 Network
라우터는 IP주소를 가진다.

![[Pasted image 20241106154411.png|600]]

![[Pasted image 20241106154654.png|600]]
라우터는 NetId만 보고 라우팅을 한다.

## Finding a network address using the default mask
![[Pasted image 20241106155637.png|500]]


![[Pasted image 20241106155836.png|500]]
이렇게 하면 너무 불편하다

![[Pasted image 20241106160219.png|500]]
라우팅할 때 grouping을 해서 네트워크의 네트워크를 두고, 이것을 서브넷이라고 한다.

![[Pasted image 20241106160529.png|500]]
예를 들어 860 -> 학교 번호 7 -> 단과대학을 나타내는 번호라고 가정하자.
7이 소프트웨어 융합학과라면, 각 단과대는 1000개의 전화번호를 쓸 수 있고, 단과대는 최대 10개가 가능하다.

만약 서브넷이 네 개 필요하다면 00,01,10,11로 서브넷을 나누어 각 네트워크의 시작 주소로 활용한다.
따라서 141.14.0.0, 141.14.64.0, 141.14.128.0, 141.14.192.0 는 각 서브넷의 시작 주소이다.

# Classless addressing
classful address의 단점: 128은 너무 크고, 65200은 너무 크다.
그래서 근본적인 문제를 해결하기 위해 클래스 자체를 없앴다. => Classless addressing

![[Pasted image 20241121111207.png|500]]
classful address는 큰 block이 앞에 오지만, classless address는 그런 순서가 필요없다.
두 단계로 끊어 읽는 것은 동일하지만, 예전에는 byte 단위로 끊어 읽던 것을 이제는 bit 단위로 끊어 읽기 시작한다.
그래서 구분을 위해 prefix, suffix라는 단어를 사용하기 시작한다.
![[Pasted image 20241121111508.png|400]]
prefix의 길이는 1에서 32까지 간다.
## Slash notation
![[Pasted image 20241121111912.png|300]]
class가 없기 때문에, slash를 하고 prefix 길이를 같이 써줘야 한다.
클래스리스 주소 지정에서는 항상 slash notation이 들어가야 한다.
# Special Addresses
## all-zero address
보통 컴퓨터는 고정 IP가 아닌 유동 IP를 사용하는데, 이 때
IP를 할당해주는 것이 DHCP 서버이다.
 DHCP에게 내가 어떤 주소를 쓰면 되는지 물어볼 때 할당해주는 것이 DHCP 서버이다. (공유기가 이 역할을 한다.)
 자신의 IP주소도 모르고 DHCP주소도 모르기 때문에, 묻는 IP를 0.0.0.0으로, Destination IP를 255.255.255.255로 보내서 IP를 할당 받는다.
![[Pasted image 20241121171148.png|300]]

## loopback address
서버에서 테스트를 하고 싶을 때, 한 컴퓨터 안에서 주고 받는 주소
![[Pasted image 20241121171014.png|300]]
## limited broadcast address
![[Pasted image 20241121171023.png|400]]
destination IP address를 255.255.255.255로 보낸 주소는 broadcast주소라고 한다.
보내게 되면 같은 network에 모든 IP에 전송한다.
라우터는 broadcast address를 block해야 한다. (block하지 않으면 계속 라우터를 통해 다른 곳으로 전송될 수 있기 때문에.)
따라서 limited broadcast address라 한다.
## directed broadcast address
![[Pasted image 20241121172321.png|400]]
suffix가 모두 1인 경우 directed broadcast address라 하며, 다른 라우터에게 보낼 데이터가 있을 때 directed broadcast address를 이용하여 보낸다.
## 사설망 주소
![[Pasted image 20241121172942.png|500]]
IP주소가 사설망 주소인지, 아닌지 구분하기 어렵기 때문에 사설망 주소를 따로 분리해두었다.
## NAT (network address translation)
사설망을 쓰기 위해 꼭 필요한 것
![[Pasted image 20241125142609.png|400]]
라우터도 network에 의해 IP 주소를 할당받는다.
밖에서 사용할 고유한 public IP (138.76.29.7)
![[Pasted image 20241125142650.png|400]]
1. 10.0.0.1이 128.119.40.186에 접속하고 싶은 상황
2. 그렇다면 10.0.0.1이라는 주소를 자신의 주소(라우터의 public IP 주소)로 변경한다.
	1. 사설망 내에 같은 목적지를 원하는 컴퓨터가 있을 수 있으므로 포트번호와 사설망 주소를 테이블로 관리한다.
	2. 10.0.0.1 3345라는 것을 저장하기 위해 사용하지 않는 port 번호를 저장해둔다.
	3. 그 후 패킷의 IP주소를 바꿔 보낸다.
3. 응답 패킷이 138.76.29.7 5001로 응답패킷이 온다면
4. 라우터는 NAT translation table을 찾아보고 원래 패킷이 가야할 10.0.0.1 3345로 보내준다.
NAT를 사용하면 IP주소 한 개로도 여러 네트워크를 꾸밀 수 있게 된다.
### port forwarding
admin이 공유기에 들어가서 8001 -> 192.168.0.1 3456으로 보내달라고 설정하는 것
(NAT 장비는 port forwarding을 자동으로 해주는 장치)


