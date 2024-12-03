라우터가 라우팅 테이블을 어떻게 만드는지에 대한 내용
## Autonomous systems
![[Pasted image 20241130174030.png|550]]
AS 안에서는 최단 경로를 우선으로 하여 테이블을 구성하지만, AS끼리 (inter-system connections)에서는 최단 경로가 아니라 rule base로 설정을 한다. 
라인은 월 별로 정산을 진행하는데, 상대적으로 월 사용료가 비싸거나 안전하지 않다면 최단 경로가 아닌 다른 경로를 선택할 수도 있다.

## routing protocols
![[Pasted image 20241130174657.png|500]]
## Distance vector에서 사용되는 벨만-포드 알고리즘
![[Pasted image 20241130174834.png|500]]

![[Pasted image 20241130175049.png|550]]
### Initialization of tables in distance vector routing
![[Pasted image 20241130175348.png|550]]
자신은 자신과 direct하게 연결되어 있는 정보만을 가지고 initial table을 만든다.
B는 A로 갔을 때 E를 A로 가는 비용 5와 A에서 D로 가는 비용 3을 더하면 8이 된다는 사실을 확인하고 자신의 라우팅 테이블에 적어놓는다.
만약, 더 비용이 적은 경로가 감지되면 그 경로로 업데이트한다.

![[Pasted image 20241130175803.png|550]]
A가 C의 테이블 정보를 받아 자신의 라우팅 테이블을 업데이트하고 싶어하는 상황
C의 테이블과 A의 테이블을 더한 임시 테이블을 만들고 자신의 원래 테이블과 비교하여 작은 값으로 업데이트 한다.
이 과정이 계속 일어나고 자신과 연결된 곳과 계속 전달된다.
여기서 C가 보내는 테이블은 cost가 더 크더라도 업데이트를 해야한다. 그렇지 않으면 네트워크 상황이 바뀌어서 cost가 더 커지는 상황이 발생했을 때 최신 정보가 반영이 안되는 상황이 발생할 수 있다. 
=> 따라서, next로 부터 to에 대한 정보를 받을 때는 cost가 내가 가지고 있는 것보다 크더라도 업데이트한다.
=> next가 다를 경우에는 비용이 작은 것으로, next가 같을 경우에는 비교하지 않고 새로 받은 테이블로 업데이트한다.

![[Pasted image 20241130180145.png|500]]

### 그렇다면, 어떻게 테이블을 주고 받을까?
![[Pasted image 20241130180247.png|550]]
하나씩 전달한다.
![[Pasted image 20241130180441.png|500]]
최종 테이블
## Two node instability

![[Pasted image 20241203222643.png|550]]
Before failure: X로 가기 위해 B는 X까지의 Cost를 1 + 1로 업데이트함
After failure: A에서 X로 가는 길이 끊어져 A에서 X로 가는 Cost를 무한으로 세팅함
After A receives update from B: B가  테이블을 업데이트하면서 A 테이블을 업데이트한다.
After B receives update from A: A가 테이블을 업데이트하면서 B 테이블을 업데이트한다.
이처럼 A와 B 사이에 Loop가 발생하며 cost가 점차 증가하는 상황하여 maximum 값에 다다라서야 Loop를 감지할 수 있는 문제가 생긴다.
#### 테이블은 언제 업데이트 되는가?
1. 주기적으로 업데이트 된다. (보통 2분)
2. 본인의 테이블이 수정이 되면 교환한다.
### 해결 방법
1. 최댓 값을 16으로 설정한다. (내부망에서 16은 큰 숫자이며 보통 16을 넘지 않기 때문)
2. split horizon - 라우터 A에게는 A가 next로 되어 있는 라우팅 정보는 보내지 않는다. 
	1. 문제 - X까지 가는 길을 몰라서 안 보내주는 것인지, 아는데 2번의 규칙으로 안 보내는 것인지 모른다.
3. split horizon & poison reverse - 라우터 A에게는 A가 next로 되어 있는 라우팅 정보를 cost를 maximum으로 해서 보낸다.
	1. B와 A는 direct로 연결되어 있고, X도 A와 연결되어 있어서 B는 X에 대한 정보를 알텐데, cost를 maximum으로 해서 보낸다면, split horizon으로 인해 보내지 못한다는 것을 알 수 있다.

## Three-node instability
![[Pasted image 20241203224856.png|500]]
X-A가 끊어져 A가 B와 C에게 정보를 전송하는데, C로 가는 패킷이 없어진 상황 (transport layer가 아닌 network layer이므로 신뢰성 보장을 할 수 없다.)
이 때 C가 주기적으로 정보를 보내게 되면, 

