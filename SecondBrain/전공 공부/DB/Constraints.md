데이터의 일관성을 유지하기 위한 장치
# Constraints 종류
## implicit constraints
- relational data model 자체가 가지는 constraints
- ex) relation 내에서는 같은 이름의 attribute를 가질 수 없다
## schema-based constraints (explicit constraints)
- 주로 DDL을 통해 schema에 직접 명시할 수 있는 constraints
### domain constraints
- attribute의 value는 해당 attribute의 domain에 속한 value여야 한다.
	- ex) age라는 attribute에 음수가 들어가면 안된다.
### key constraints
- 서로 다른 tuples는 같은 value의 key를 가질 수 없다.
### NULL value constraint
- attribute가 NOT NULL로 명시됐다면 NULL을 값으로 가질 수 없다.
### entity integrity constraint
- primary key는 value에 NULL을 가질 수 없다.
### referential integrity constraint
- FK와 PK와 도메인이 같아야 하고 PK에 없는 values를 FK가 값으로 가질 수 없다.

# 데이터베이스 constraints 정의
## PRIMARY KEY
![[Pasted image 20250119221504.png|400]]
## UNIQUE
![[Pasted image 20250119221553.png|400]]
## DEFAULT
![[Pasted image 20250119221734.png|400]]
## CHECK
![[Pasted image 20250119221806.png|400]]
## FOREIGN KEY
![[Pasted image 20250119221425.png|400]]
참조하고 있던 값이 삭제되거나 업데이트 되었을 때 FK는 어떻게 할지 선언해줄 수 있다.
