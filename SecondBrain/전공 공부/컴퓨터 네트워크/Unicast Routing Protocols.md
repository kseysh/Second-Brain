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

![[Pasted image 20241130180145.png|500]]

### 그렇다면, 어떻게 테이블을 주고 받을까?
![[Pasted image 20241130180247.png|550]]
하나씩 전달한다.
![[Pasted image 20241130180441.png|500]]
최종 테이블
## Two node instability



