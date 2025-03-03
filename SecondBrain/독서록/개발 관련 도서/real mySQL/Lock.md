# MySQL 엔진 잠금
모든 스토리지 엔진에 영향을 미친다.
## 글로벌 락
한 세션에서 글로벌 락을 획득하면 다른 세션에서 대부분의 DDL, DML의 사용이 어렵다.
### 백업 락
특정 세션에서 백업 락을 획득하면 모든 세션에서 테이블의 스키마나 사용자의 인증 관련 정보를 변경할 수 없게 된다.
- 테이블 등 모든 객체 생성 및 변경, 삭제 불가
- 사용자 관리, 비밀번호 변경 불가
백업 시에 글로벌 락은 너무 강력한 락이기 때문에 일반적인 테이블의 데이터 변경은 가능한 백업락을 사용한다.
## 테이블락
개별 테이블 단위로 설정되는 락
InnoDB에서는 DDL 쿼리의 경우에만 영향을 미친다.
## 메타데이터 락
데이터베이스 객체의 이름이나 구조를 변경하는 경우 획득하는 락
운영하며 테이블 구조를 변경하고 싶을 때 사용할 수 있다 (과정 p.165-166)
# InnoDB 스토리지 엔진 잠금
스토리지 엔진간 상호 영향을 미치지는 않는다.
## 레코드 락
InnoDB는 레코드 락을 걸 때, 레코드 단위가 아닌 인덱스의 레코드 락을 건다.
만약 인덱스가 없더라도 내부적으로 자동 생성된 클러스터 인덱스(보조 인덱스가 포인터를 이용해 참조하는 인덱스)를 이용해 잠금을 설정한다.
InnoDB에서는 대부분 보조 인덱스를 이용한 변경작업은 Next key lock 또는 Gap lock을 사용하지만 PK 또는 unique 인덱스에 의한 변경 작업에서는 레코드 자체에 대해서만 락을 건다.
## 갭 락
레코드 자체가 아니라 레코드와 바로 인접한 레코드 사이의 간격만을 잠그는 것
=> 레코드와 레코드 사이의 간격에 데이터가 추가되는 것을 제어한다 (이를 통해 repeatable read에서 phantom read를 막을 수 있다.)
## 넥스트 키 락
레코드 락과 갭 락을 합쳐 놓은 형태의 잠금
갭 락은 그 자체를 사용하기 보다는 넥스트 키 락의 일부로 자주 사용된다.
## 자동 증가 락
index의 AUTO_INCREMENT 칼럼이 중복되지 않고 인덱스를 생성하기 위해 생성된다.
innodb_autoinc_lock_mode라는 시스템 변수를 이용해 자동 증가 락의 작동 방식을 변경할 수 있다.

# 인덱스와 잠금
InnoDB의 레코드 락은 인덱스를 이용해 잠금을 처리한다.
따라서, 인덱스를 잘못 설정하면 인덱스에 걸려있는 많은 데이터들이 같이 잠금될 수 있기 때문에 인덱스를 유의해서 설정해야 한다.
만약 테이블에 인덱스가 하나도 없다면 테이블을 풀 스캔하며 모든 레코드를 잠그게 된다.
=> UPDATE나 DELETE 문장 실행에서 사용할 수 있는 인덱스가 있는지 잘 확인해야 한다.