모든 aws 서비스는 api 호출

![[Pasted image 20260120111046.png|300]]
Region > CPC > Subnet
Region > AZ > Subnet
=> S3, RDBㅇ
## Region 
- AWS 서비스를 제공하는 도시의 이름과 코드
- 하나의 리전은 최소 2개 이상의 AZ를 제공 (가용성을 위함)
## Availablility Zone
- 가용성을 제공하기 위한 논리적인 데이터센터
- 하나 이상의 데이터 센터를 가짐
## Edge Location
- 리전 외의 추가 데이터 센터
- CDN의 중요한 구성요소로 사용되며, 사용자와 물리적으로 가까운 위치에서 콘텐츠를 제공함으로써 Latency를 줄이고 성능을 향상

## EC2
AWS에서 제공하는 Virtual Machine
## VPC
NAT 게이트웨이를 통해 내부 IP를 만들어 사용한다.

EC2를 실행하기 위해 정의하는 가상의 사설네트워크 망 (외부인이 접근할 수 없는 망)
여러 리전에 생성될 수 없지만, 여러 AZ에 생성될 수는 있다.
## Subnet
VPC의 IP를 더 작은 그룹으로 분할하는 역할을 하는 추상적인 개념
public / private로 만든다.
## Internet GateWay (IGW)
VPC로 들어올 수 있도록 하는 NAT

### public
인터넷 연결 가능
### private
인터넷 연결 불가능
Internet GateWay 경로가 없으며, 필요시 NAT Gateway를 통해 우회 접속해야 함
안전하게 DB같은 경우 private subnet을 이용
public보다 private 서브넷을 많이 활용하는 것이 권장됨.
## Route Table

