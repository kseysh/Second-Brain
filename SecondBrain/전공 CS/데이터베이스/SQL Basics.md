## Create Table
create table r (A1 D1, A2 D2, ..., An Dn,...,)
r: relation 이름
A: 속성 이름
D: A의 데이터 타입
### Insert
`insert into instructor values (‘10211’, ’Smith’, ’Biology’, 66000);`
## Integrity Constraints
primary key (A1, ..., An )
foreign key (Am, ..., An ) references r
not null
#### example
![[Pasted image 20250320140540.png|300]]
## drop table
테이블 자체 삭제
## alter table 
### alter table r add A D

### alter table r drop A
컬럼 삭제는 많은 DB에서 지원하지 않는다.

## select 절
- 중복 제거 예제
select distinct dept_name
from instructor
- 중복 삭제를 하지 않으려면,
select all dept_name
from instructor
## natural Join 절
모든 속성에서 같은 이름을 가지고 있는 tuple을 매치해준다.
select name, course_id
from instructor natural join teaches;


DB는 미리 공부하고 거기 가기 전까지 혼자 다른 공부하자... 