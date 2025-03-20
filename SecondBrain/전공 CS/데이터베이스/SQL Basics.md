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
A: 관계 r에 추가될 속성의 이름
D: A의 도메인 
관계의 모든 튜플은 새 속성의 값으로 null이 할당
### alter table r drop A
컬럼 삭제
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
## Rename Operations (=as)
## String Operations
%: 어느 substring이던 매치
\_: 어느 character이던 매치
ex) where name like '%dar%'
100%를 매치하려면, like '100 \\%' escape '\\'
## between
ex) where salary between 90000 and 100000
## Tuple comparison
where (instructor.ID, dept_name) = (teaches.ID, ‘Biology’);
## Set Operations
Set의 작업들은 모두 자동으로 중복을 제거한다.
만약, 중복을 제거하고 싶다면, union all 같이 all을 붙여 사용한다.

ex)
(select course_id from section where sem = ‘Fall’ and year = 2009)
union
(select course_id from section where sem = ‘Spring’ and year = 2010)
### union
합집합
### intersect
교집합
### except
차집합
