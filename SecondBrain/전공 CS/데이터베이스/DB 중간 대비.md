###### 파일 시스템의 문제
- 데이터 중복성과 불일치 (정규화)
- 원자성 보장 x (Atomic)
- 무결성 보장 x (Consistency)
- 데이터의 고립 및 분리 (Isolation)
- 동시성으로 인한 데이터 불일치 (Isolation)
- 접근 제어의 어려움
- 데이터 접근의 어려움
###### DB 세 가지 layer 추상화
- physical level
	- 실제 데이터가 저장되는 곳
- logical level
	- 어떤 속성이 있는가
	- 어떤 데이터를 가지고 있어야 하는가
- view level
	- 일반 사용자들이 쉽게 이해할 수 있는 개념들로 이뤄진 레벨
	- logical level의 부분집합일 수도 있고
	- view level이 새로 생성될 수도 있음 (다대다 만들 때 만드는 관계같은 것)
###### Physical Data Independence
- 물리적 스키마를 변경해도 논리적 스키마를 변경하지 않을 수 있는 능력
- DB를 두 대로 변경하던, 여러 파일로 저장하던 table은 변하지 않는다.
###### 스키마의 뜻과 스키마의 종류
- 데이터 베이스의 논리적 구조
- physical schema
	- physical level에서의 schema
- logical schema
	- logical level에서의 schema
###### Instance
특정 시점에서의 데이터베이스 실제 내용
###### Super key
어떤 tuple을 identify 하는데 충분한 키
사실 R 자체도 super key다
###### candidate key
super key 중 minimal한 key
null이 될 수 있다
ex) (Id), (학과번호, 학과별 수업 번호) 둘 다 나누게되면 식별불가능해지므로 둘 다 minimal하다고 볼 수 있다.
###### PK
candidate key 중 선택된 key
###### 제약 조건을 가질 때 / 가지지 않을 때 표기
![[Pasted image 20250318133755.png|200]]
화살표를 쓰냐(제약조건을 가짐) 안쓰냐(논리적으로만 연결됨)
###### SQL은 무슨 방식인지?
Relation Algebra로, Functional 방식
###### select tuples with A=B and D > 5를 Relational Algebra로
![[Pasted image 20250405224447.png]]
###### selecting instructor tuples with salary greater than 85000을 Relational Algebra로
𝜎<sub>salary>=85000</sub>(instuctor)
###### Select A and C를 Relational Algebra로
![[Pasted image 20250405224626.png]]
###### Selecting attributes ID and salary from the instructor relation Relational Algebra로
∏<sub>ID,salary</sub>(instructor)[[]]
###### Theta Join을 다르게 표현
![[Pasted image 20250318142719.png|300]]
###### Theta join 특징
Theta join자체에는 Projection(select)를 진행하지 않음
###### Union 특징
중복 제거[[]]
###### ID가 12121인 교수보다 더 많은 급여를 받는 교수들의 ID 찾기 X, Theta join 표현
![[Pasted image 20250405230525.png]]
rho는 바깥으로 해도 됨
$$
\pi_{i.ID} \left( \rho_i(\text{instructor}) \Join_{i.salary > j.salary \wedge j.ID = 12121} \rho_j(\text{instructor}) \right)
$$
###### numeric(p, d)
Fixed point number, p: 전체 자리수, d: 소수점 오른쪽 자리수
###### real, double precision
부동 소수점 숫자 타입
###### float(n)
부동 소수점 타입, n은 정밀도 자리수로 n에 따라 real 또는 double precision으로 매핑
###### create
create table r (A1 D1, A2 D2, ..., An Dn,...,)
###### insert
`insert into instructor values (‘10211’, ’Smith’, ’Biology’, 66000);`
###### pk 제약 생성
primary key (A1, ..., An )
insert에서 Domain쪽에 primary key라고 적기
###### fk 제약 생성
foreign key (Am, ..., An ) references r
###### 컬럼 추가
alter table r add A D
###### 컬럼 삭제
alter table r drop A
###### select시 중복 삭제 안하려면
select all
###### dept_name이 Comp. Sci dept이고, salary>8000인 instructor의 name 찾기
```sql
select name
from instructor
where dept_name ='Comp. Sci.' and salary > 80000
```
###### dept_name이 Comp.Sci이고고 section.course_id와 course.course_id가 같은 course ID, semester, year, title 찾기 (x 이용)
```sql
select section.course_id, semester, year, title
from section, course
where section.course_id = course.course_id and dept_name = 'Comp. Sci.'
```
###### name에 dar가 들어가는 교수의 이름 찾기
```sql
select name
from instructor
where name like '%dar%'
```
###### name에 100%가 들어가는 교수의 이름 찾기
```sql
select name
from instructor
where name like '%100\%%' escape '\'
```
###### salary가 90000 ~ 100000인 교수 이름 찾기
```sql
select name
from instructor
where salary between 90000 and 100000
```
###### tuple comarison
```sql
select name, course_id
from instructor, teaches
where (instructor.ID, dept_name) = (teaches.ID,'Biology’);
```
###### set operation의 종류와 특징
- union
합집합
- intersect
교집합
- except
차집합

set 작업은 모두 자동으로 중복을 제거한다
###### Null과 unknown
null은 어떤 수학적 연산을 해도 null이라는 결과가 나온다.
null은 어떤 비교 연산을 해도 unknown이라는 결과가 나온다.
unknown은 null과 비슷하게 `is unknown`이라는 연산을 사용한다.
###### 각 부서의 평균 월급 찾기
```sql
select dept_name, avg (salary) as avg_salary
from instructor
group by dept_name;
```
###### 2010년 봄 학기에 강의를 가르치는 강사의 총 수 찾기
```sql
select count (distinct ID)
from teaches
where semester = 'Spring' and year = 2010;
```
###### Aggregate Function의 특징
Aggregate Function 외부의 select 절의 속성은 목록별로 그룹화해야 한다.
```sql
select dept_name, ID, avg (salary) # ID는 그룹화 되지 않은 속성이므로 에러가 발생한다.
from instructor
group by dept_name;
```
###### 부서의 평균 월급이 42000을 넘는 부서의 이름과 평균 월급 찾기
```sql
select dept_name, avg (salary)
from instructor
group by dept_name
having avg (salary) > 42000;
```
having -> group by로 가져올 때의 조건을 설정하는 절
###### having vs where
having = 그룹 형성 후 적용
where = 그룹 형성 전 적용
###### Aggregation Function과 Null
SUM() 함수는 집계 대상에 모든 값이 NULL일 경우, NULL을 반환
COUNT(\*)를 제외한 대부분의 집계 함수는 NULL 값을 무시하며,
집계 대상이 비어 있을 경우(조건에 해당하는 행이 없을 경우),
COUNT는 0을 반환하고, 나머지 집계 함수들은 NULL을 반환한다.
###### 2009년 가을과 2010년 봄에 제공된 강의의 course_id 찾기 in, nested subquery를 이용해서
```sql
select distinct course_id
from section
where semester = ’Fall’ and year= 2009 and
	course_id in (select course_id
		from section
		where semester = ’Spring’ and year= 2010);
```
###### 2009년 가을과 2010년 봄에 제공된 강의의 course_id 찾기 nested subquery, exists를 이용해서
```sql
select course_id
from section as S
where semester = ’Fall’ and year= 2009 and
	exists (select *
		from section as T
		where semester = ’Spring’ and year= 2010
			and S.course_id= T.course_id);
```
###### 2009년 가을에 제공되고, 2010년 봄에 제공되지 않은 강의 찾기 not in
```sql
select distinct course_id
from section
where semester = ’Fall’ and year= 2009 and
	course_id not in (select course_id
		from section
		where semester = ’Spring’ and year= 2010);
```
###### ID 10101을 가진 강사가 가르친 과정 섹션을 수강한 학생을 중복을 제외한 총 수 찾기
```sql
select count (distinct ID)
from takes
where (course_id, sec_id, semester, year) in // 4개가 다 pk라 4개 다 써줘야 함
	(select course_id, sec_id, semester, year
	from teaches
	where teaches.ID= 10101);
```
###### some
서브쿼리에서 반환된 값 중 하나 이상과 비교 조건을 만족하면 TRUE ([[ANY]]와 같은 역할)
###### 생물학 부서의 일부 (적어도 한 명) 강사의 급여보다 더 큰 급여를 받는 강사의 이름을 찾기
```sql
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept name = 'Biology';
```
###### 생물학 부서의 일부 (적어도 한 명) 강사의 급여보다 더 큰 급여를 받는 강사의 이름을 찾기 (Set 비교 활용)
```sql
select name
from instructor
where salary > some (select salary
					from instructor
					where dept name = 'Biology');
```
###### all
![[Pasted image 20250320172756.png|200]]
###### 2009년 가을 학기와 2010년 봄 학기 두 학기 모두에 개설된 모든 course_id 찾기(exists 사용)
```sql
select course_id
from section as S
where semester = ’Fall’ and year= 2009 and
	exists (select *
		from section as T
		where semester = ’Spring’ and year= 2010
			and S.course_id= T.course_id);
```
###### 생물학과에서 제공되는 모든 과목을 수강한 모든 학생의 ID와 이름을 중복을 제거하고 찾기 (not exists 사용)
```sql
select distinct S.ID, S.name
from student as S
where not exists ( (select course_id
		from course
		where dept_name = 'Biology')
	except
		(select T.course_id
		from takes as T
		where S.ID = T.ID));
```
(모든 생물학과 과목) - (학생이 들은 과목) = 공집합이면 그 학생은 모든 생물학과 과목을 수강한 것
###### unique
subquery에 중복된 결과가 있는지 테스트
###### 2009년에 최대 한 번 제공되었던 모든 Course의 course_id 찾기 (두 번이면 안 됨)
```sql
select T.course_id
from course as T
where unique (select R.course_id
		from section as R
		where T.course_id= R.course_id
			and R.year = 2009);
```
###### Derived Relation이란?
SQL 쿼리 안에서 FROM 절에 사용되는 서브쿼리로, 일시적인 이름(alias)을 붙여서 마치 테이블처럼 다룰 수 있는 것
###### 평균 급여가 42,000달러 이상인 부서의 평균 강사 급여를 찾기 (Derived Relation 사용)
```sql
select dept_name, avg_salary
from (select dept_name, avg (salary) as avg_salary
	from instructor
	group by dept_name)
where avg_salary > 42000;
```

```sql
select dept_name, avg_salary
from (select dept_name, avg (salary)
	from instructor
	group by dept_name) as dept_avg (dept_name, avg_salary)
where avg_salary > 42000;
```
###### with
with 절이 발생하는 쿼리에 대해서만 정의를 사용할 수 있는 temporary view를 정의하는 방법을 제공
with name () as select...
###### 모든 부서 중 최대 예산을 갖는 부서의 최대 예산 값 찾기 (with max_budget 활용)
```sql
with max_budget (value) as
	(
	select max(budget)
	from department
	)

select budget
from department, max_budget
where department.budget = max_budget.value;
```
###### 모든 부서의 총 급여가 총 급여의 평균보다 큰 모든 부서를 찾기 (with dept_total, dept_total_avg 활용)
```sql
with dept_total (dept_name, value) as
	(select dept_name, sum(salary)
	from instructor
	group by dept_name),
dept_total_avg(value) as
	(select avg(value)
	from dept_total)

select dept_name
from dept_total, dept_total_avg
where dept_total.value >= dept_total_avg.value;
```
###### 모든 부서의 이름과 교수의 수 찾기 (scalar subquery 사용)
```sql
select dept_name,
	(select count(*)
	from instructor
	where department.dept_name = instructor.dept_name)
	as num_instructors
from department;
```
###### 모든 교수 삭제
```sql
delete from instructor
```
###### Finance 부서의 모든 교수 삭제
```sql
delete from instructor
where dept_name= 'Finance';
```
###### - Watson 건물에 있는 학과(department)에 소속된 교수들(instructor)을 삭제
```sql
delete from instructor
where dept_name in (select dept_name
		from department
		where building = 'Watson');
```
###### delete from instructor where salary< (select avg (salary) from instructor) 의 문제
삭제하는 동안 평균 급여가 변경되므로 avg(salary)값이 다시 계산되었을 때 바뀌게 됨

먼저 평균 급여를 계산하고, 그 값보다 적은 급여를 가진 튜플(행)을 미리 따로 저장하여 저장한 것만 삭제
###### 급여가 강사의 평균 급여보다 적은 모든 강사를 삭제
```sql
DELETE FROM instructor
WHERE ID IN (
        SELECT ID
        FROM instructor
        WHERE salary < (
            SELECT AVG(salary)
            FROM instructor
		)
);
```
###### insert 문
```sql
insert into course
values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
```
value x values O
```sql
insert into course (course_id, title, dept_name, credits)
values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
```
###### 모든 instructor를 student 테이블로 옮기되, tot_creds(총 이수 학점)는 0으로 설정
```sql
insert into student
	select ID, name, dept_name, 0
	from instructor
```
###### 위 코드의 순서
select 문이 다 실행되고 나서야 insert문이 실행된다.
###### 급여가 $100,000 이상인 강사의 급여를 3% 인상하고, 다른 모든 강사는 5% 인상을 받기 (case 사용)
```sql
update instructor
set salary = salary * 1.03
where salary > 100000;
---
update instructor
set salary = salary * 1.05
where salary <= 100000;
```

```sql
update instructor
set salary = case
	when salary <= 100000 then salary * 1.05
	else salary * 1.03
	end
```
###### 각 학생의 tot_cred를, F학점이 아니고 NULL이 아닌 성적을 받은 과목들의 총 학점 합으로 업데이트
```sql
update student S
set tot_cred = ( select sum(credits)
	from takes natural join course
	where S.ID= takes.ID and
		takes.grade <> ’F’ and
		takes.grade is not null);
```
표준 SQL에서는 <>가 공식적인 같지 않음 연산자임

###### 위 문제에서 sum(credit)이 null인 학생들은 tot_creds를 0으로 설정
```sql
update student S
set tot_cred = ( select case
							when sum(credits) is not null then sum(credits)
							else 0
						end
	from takes natural join course
	where S.ID= takes.ID and
		takes.grade <> ’F’ and
		takes.grade is not null);
```
###### views
모든 사용자가 전체 논리 모델을 보는 것이 바람직하지 않은 경우에 사용
특정 사용자에게 특정 데이터를 숨기기 위한 메커니즘 제공
개념적 모델에는 없지만 사용자에게 가상 릴레이션으로 보이는 릴레이션
```sql
create view v as <쿼리 표현식>
```
###### 급여 정보 없이 강사 정보를 보여주는 뷰, 뷰를 이용해 생물학과 소속 강사들의 이름 조회
```sql
create view faculty as  
select ID, name, dept_name  
from instructor;
```

```sql
select name  
from faculty  
where dept_name = 'Biology';
```
###### 학과별 총 급여를 보여주는 뷰 departments_total_salary(dept_name, total_salary)
```sql
create view departments_total_salary(dept_name, total_salary) as  
select dept_name, sum(salary)  
from instructor  
group by dept_name;
```
###### 2009년 가을 학기에 열렸던 물리학과(Physics)의 모든 강의에 대해 course_id, sec_id, building, room_number를 모아 physics_fall_2009라는 뷰(view)를 생성, 
```sql
create view physics_fall_2009 as  
select course.course_id, sec_id, building, room_number  
from course, section  
where course.course_id = section.course_id  
	and course.dept_name = 'Physics'  
	and section.semester = 'Fall'  
	and section.year = 2009;
```
###### physics_fall_2009 뷰에서 건물이 ‘Watson’인 강의만 골라 강의 ID와 강의실 정보를 담은 physics_fall_2009_watson 뷰를 생성
```sql
create view physics_fall_2009_watson as  
select course_id, room_number  
from physics_fall_2009  
where building = 'Watson';
```
##### 아래 코드의 문제
```sql
create view instructor_info as
select ID, name, building
from instructor, department
where instructor.dept_name = department.dept_name;
```
위 뷰에 아래처럼 insert를 시도하면,
```sql
insert into instructor_info values ('69987', 'White', 'Taylor');
```
###### A
Taylor 건물에 여러 개의 학과가 있다면 어떤 학과로 지정해야 할지 모르며,
Taylor 건물에 어떤 학과도 없다면 삽입이 불가능한 문제가 발생한다.
###### 일반적으로 SQL view가 updatable하다고 판단되는 경우
- from 절에 하나의 relation만 포함되어 있을 때
- select절에 relation의 속성 이름만 포함되어 있고, 표현식, 집계 함수, distinct등이 포함되어 있지 않을 때
- select 절에 포함되지 않은 속성이 nullable할 때
- group by나 having 절이 쿼리에 포함되어 있지 않을 때
##### 아래 코드의 문제
```sql
create view history_instructors as
select *
from instructor
where dept_name = 'History';
```
위 뷰에서 아래 sql을 실행하면,
```sql
insert into history_instructors values ('25566', 'Brown', 'Biology', 100000);
```
###### A
where절의 조건 (즉, dept_name = 'History)을 만족하지 않기 때문에 insert가 차단된다.
with check option 절이 뷰 절의 마지막에 붙어 있을 경우, 뷰 정의의 조건을 만족하지 않는 행의 삽입이나 갱신을 방지한다.
###### check(P)
각 튜플이 반드시 만족해야 하는 조건 P를 가짐
###### semester가 반드시 Fall, Winter, Spring, Summer중 하나여야 할 때
```sql
create table section (
  course_id varchar(8),
  sec_id varchar(8),
  semester varchar(6),
  year numeric(4,0),
  building varchar(15),
  room_number varchar(7),
  time_slot_id varchar(4),
  primary key (course_id, sec_id, semester, year),
  check (semester in ('Fall', 'Winter', 'Spring', 'Summer'))
);
```
###### 관련 데이터 삭제 / 변경시 자동으로 해당 참조도 삭제 / 변경하려면,
```sql
create table course (
  ...
  dept_name varchar(20),
  foreign key (dept_name) references department
    on delete cascade
    on update cascade,
  ...
);
```
##### 아래 문제 해결
```sql
create table person (
  ID char(10),
  name char(40),
  spouse char(10),
  primary key (ID),
  foreign key (spouse) references person
);
```

이 상황에서 배우자인 John과 Mary를 동시에 추가하려면, 
```sql
insert into person values ('10101', 'John', '11111');
insert into person values ('11111', 'Mary', '10101');
```
서로 존재하지 않는 사람을 참조하게 되어 제약 조건 위반이 발생할 수 있다.
###### spouse 컬럼이 person 테이블을 참조하는 외래 키 제약 조건 정의
```sql
constraint spouse_ref foreign key (spouse) references person
```
###### spouse_ref 제약 조건을 지연 모드(deferred)로 설정
```sql
set constraints spouse_ref deferred;
```
###### index 생성
create index id_idx on student(id)
###### composite Attributes란?
![[Pasted image 20250408141008.png|500]]
##### 무슨 관계인지
![[Pasted image 20250408142911.png|300]]
![[Pasted image 20250408142853.png|300]]
![[Pasted image 20250408142922.png|300]]
![[Pasted image 20250408142933.png|300]]
###### A
one to many
one to one
many to one
many to many
###### Total Participation이란?
이중 선으로 표시되며 엔티티 집합의 모든 엔티티가 관계 집합에 최소한 하나는 참여해야 한다.
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
###### Q
A
