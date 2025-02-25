# Forwarding
라우팅 테이블을 보고 next hop으로 가게끔 interface에 패킷을 가져다 두는 것
forwarding -> 라우팅 테이블도 같이 일을 한다.
목적지 주소를 보고 전달하는 것이 기본이고, Label을 보고 전달하기도 한다.
## Next-hop method
![[Pasted image 20241125145904.png|400]]
a. 경로 중심의 라우팅 테이블
A에서 B로 가는 모든 경로를 라우팅 테이블에서 저장해둔다
b. next hop 기반의 라우팅 테이블
A에서 B로 갈 때 next hop의 경로만 라우팅 테이블에서 저장해둔다.
## Network-specific method
![[Pasted image 20241125150145.png|400]]
N2가 class A라면 라우팅 테이블은 N2에 대해서만 1600만개의 주소를 저장하고 있어야 한다.
그래서 사실 S 입장에서는 A,B,C,D가 어디있는지는 몰라도 된다.
S는 자신이 누구에게 패킷을 보내면 되는지에 대해서만 알면 된다.
그래서 N2를 대표하는 주소를 정해놓는 것이다.
=> 그래서 Network 주소를 mask하는 것
이렇게 되면 라우팅 테이블에서 class A의 1600만개의 주소를 단 한 개의 주소를 라우팅 테이블에 저장함으로써 절약할 수 있다.
![[Pasted image 20241125150554.png|400]]
라우팅 테이블에는 네트워크에 대한 정보를 저장할 때도 있지만, Host의 주소를 구체적으로 적을 때도 있다 => Host-specific routing
따라서 라우팅 테이블에는 네트워크 주소만 들어가는 것이 아닌 호스트 주소가 들어갈 수도 있다.
## Default routing
![[Pasted image 20241125150757.png|400]]
라우팅 테이블은 주변에 있는 정보만 가지고 있어도 된다.
그림에서 R1이 아니면 모두 R2를 거쳐서 데이터를 보내도록 한다 => Default routing
## Simplified forwarding module in *classful address without subneting*
![[Pasted image 20241125151006.png|500]]
주소를 받았을 때 라우터가 어떠한 일을 하는지?
1. 목적지 주소를 받는다.
2. class를 찾는다
3. class별로 mask를 적용한다. (class별로 라우팅 테이블을 따로 관리한다.)
4. table을 찾는다.

**direct delivery는 next-hop address가 비어있다.**
**indirect delivery는 라우터의 next-hop address가 저장되어 있다.**
class 별로 라우팅 테이블이 구성되어 있다.
## Simplified forwarding module in *classful address* *with subnetting*
![[Pasted image 20241125153812.png|400]]
서브넷이 있는 경우에는 밖에서는 몰라도 되게끔 구성한다.
네트워크 내에서는 서브넷이 몇 개가 생겼고, mask는 어떻게 되는지 다 세팅해주어야 한다. 그래야 라우터가 라우팅 테이블을 보고 다 찾아놓을 수 있다.
1. 처음에는 패킷 주소를 본다
2. subnet mask를 통해 subnet address를 찾아낸다
3. 테이블을 찾는다.

![[Pasted image 20241125154349.png|400]]
네트워크가 어떻게 구축되어 있는지 라우터는 알아야 한다.
mask를 어떻게 해야하는지 라우터는 알아야 한다.
## Simplified forwarding module in *classless address*
classful에서는 최소 3개의 컬럼만 있으면 된다 => destination, next hop, interface
하지만 classless에서는 mask라는 컬럼도 추가적으로 필요하다.

![[Pasted image 20241120153750.png|500]]
## Address aggregation
인하 안경, 인하 칼국수, 인하 반점, 인하 짬뽕은 다 다른 네트워크지만, 비슷한 위치에 있다는 공통점이 있음 (용현동임)
그러면 부산에서 가려면 굳이 구별 안해도 됨 여러 주소를 사용하지 않고 한 주소로 합칠 수 있음
=> address aggregation 

근데 인하 짬뽕이 부산으로 옮긴다면 aggregation을 못쓰는가?
no 예외처리를 할 수 있다,
인하 짬뽕만 먼저 처리하면 된다.
이후 인하짬뽕이 아닌 인하를 처리한다
![[Pasted image 20241120160011.png]]
라우팅 테이블 ->  위에서 부터 하나씩 하고 맞으면 거기서 끝난다 따라서 예외 처리를 가장 위에 써놓는다.
Longest mask matching => 가장 긴 마스크부터 매칭해본다

## Multiprotocol label switching (MPLS)
![[Pasted image 20241120160832.png|500]]
우리 동만 관리하는 택배아저씨는 ~시 ~로 ~동을 확인하지 않음 호수만 크게 작성해서 빠르게 작업할 수 있도록 함
따라서 택배 회사마다 label을 달아둔다.
MPLS가 되는 네트워크에는 label을 붙여 사용한다.
위치는 IP 헤더 앞에 

![[Pasted image 20241120161137.png|500]]
IP 주소 없고, 숫자가지고 일을 함
라우팅 테이블을 새로 만드는 것, 왜냐하면 라우팅 테이블은 탐색 시간이 n만큼 걸리기 때문

