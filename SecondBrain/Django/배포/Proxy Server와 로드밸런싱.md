[[Proxy Server]]

## Forward Proxy
프록시 서버는 크게 Forward Proxy, Reverse Proxy의 두 종류로 나뉜다.
일반적으로 부르는 프록시 서버는 Forward Proxy를 의미한다.

	client -> Forward Proxy -> Internet -> Server

이 순서로 갈 때 사용되는 proxy가 Forward Proxy이다.

### Forward Proxy의 특징

#### 1. 캐싱
[[캐싱]]이란? =>
자주 요청하는 결과 데이터를 캐시에 저장해놓고, 해당 데이터를 재요청하는 경우 서버에 재요청하지 않고 프록시 서버의 캐시에 저장해놓은 데이터를 리턴하는 방식을 의미
ex)
첫 번째 클라이언트가 서버에게 요청을 보내는 상황
![[Pasted image 20230702230245.png]]
여러개의 서버중 가요차트 정보를 제공해주는 담당 서버는 클라이언트가 원하는 정보를 아래처럼 Forward Proxy 서버와 인터넷을 통해 정보를 주고 받는다.
이 때 프록시 서버는 단순한 정보만을 제공하는 것이 아니라 캐시에다 제공한 정보를 저장한다.

![[Pasted image 20230702230602.png]]
그리고 나중에 여러 클라이언트들이 같은 정보를 요구하였을 때 프록시 서버는 캐시에 저장해놓았던 데이터를 제공해준다. 즉, 서버까지 도달하지 않고도 캐시에 저장한 데이터를 프록시 서버가 대신 제공해주게 된다.

#### 2. 익명성
익명성 : 클라이언트가 보낸 요청을 감춘다는 것
클라이언트가 서버로 직접 호출할 때는 클라이언트의 IP주소, 장비 정보, OS 정보등을 그대로 서버에게 전달하지만 Forward Proxy를 사용한다면, 클라이언트가 요청했지만 마치 Forward Proxy가 요청을 한 것 처럼 서버에게 Forward Proxy의 정보들을 전달할 수 있다.

## Reverse Proxy
Reverse Proxy는 Forward Proxy와 유사하나, Forward Proxy와 달리 인터넷 - 서버 사이에 위치한다는 점이 다르다
Forward Proxy : 클라이언트 -> Forward Proxy -> 인터넷 -> 서버
Reverse Proxy : 클라이언트 -> 인터넷 -> Reverse Proxy -> 서버

### Reverse Proxy의 특징

#### 1. 캐싱
Forward Proxy와 동일하게 캐싱이 가능하다.

#### 2. 보안
서버의 정보를 클라이언트로부터 숨길 수 있다.
클라이언트는 요청을 할 때 어떤 서버에 요청을 하는지, 또 해당 서버의 정보는 무엇인지 직접 알지 못한다. 그 대신 클라이언트 입장에서 서버인 Reverse Proxy에게 요청을 전달한다. 따라서 실제 서버들의 IP는 클라이언트로부터 안전하게 보호되며 실제 서버의 IP가 유출되지 않는다.

#### 3. [[로드밸런싱]]














