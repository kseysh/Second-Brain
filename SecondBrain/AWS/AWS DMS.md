DMS = Database Migration Service
![[Pasted image 20250312221633.png|400]]
## DMS 진행 순서
### 기존 데이터 마이그레이션
초기 적재가 완료될 때까지 변경사항은 타깃 시스템에 적용되지 않는다.
### 마이그레이션 이후 Source DB에서 발생한 변경사항을 Target DB에 적용
Source DB에서의 데이터 변화를 binlog를 통해 감지하고, 지속적인 마아그레이션을 진행한다.
### 변경된 데이터의 지속적인 복제 후 서비스 전환

## DMS 특징
1. 마이그레이션 중에도 기존에 사용하는 DB 사용가능
2. 이기종 마이그레이션 적용 가능
## DMS 관련 개념
### 복제 인스턴스
Source DB에서 Target DB로 data 이관 시 사용되는 인스턴스
### Source Endpoint
source DB와의 연결점
### Target Endpoint
target DB와의 연결점
### Schema Conversion Tool (SCT)
기존 DB 스키마를 이기종 DB 스키마로 변환할 수 있는 기능을 제공하는 서비스
### DB Migration task
