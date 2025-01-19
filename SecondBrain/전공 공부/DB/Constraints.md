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