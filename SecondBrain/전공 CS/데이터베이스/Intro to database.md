## 파일 시스템의 문제
- 데이터 중복성과 불일치
- 데이터 접근의 어려움
- 데이터의 고립 및 분리
- 무결성 보장 x
- 원자성 보장 x
- 동시성으로 인한 데이터 불일치
- 접근 제어의 어려움
## DB 추상화
[[데이터베이스]]
- physical level
	- 실제 데이터가 저장되는 곳
- logical level
	- 어떤 속성이 있는가
	- 어떤 데이터를 가지고 있어야 하는가
- view level
	- 일반 사용자들이 쉽게 이해할 수 있는 개념들로 이뤄진 레벨
	- logical level의 부분집합일 수도 있고
	- view level이 새로 생성될 수도 있음 (다대다 만들 때 만드는 관계같은 것)
## Instances, Schema
- Schema
	- 데이터 베이스의 논리적 구조
	- physical schema
		- physical level에서의 schema
	- logical schema
		- logical level에서의 schema
- Instance
	- 특정 시점에서의 데이터베이스 실제 내용
- Physical Data Independence
	- 물리적 스키마를 변경해도 논리적 스키마를 변경하지 않을 수 있는 능력
	- DB를 두 대로 변경하던, 여러 파일로 저장하던 table은 변하지 않는다.
## SQL
non-procedural language이고, 선언형 언어이다. (내부 구현은 DB가 알아서 한다 How를 설정하지 않는다)
### DDL
Data Definition Language
### DML
Data Manipulation Language
