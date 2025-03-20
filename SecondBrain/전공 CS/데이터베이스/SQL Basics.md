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
```sql
select distinct dept_name
```
from instructor
- 중복 삭제를 하지 않으려면,
```sql
select all dept_name
from instructor
```
## where절
Comp. Sci dept이고, salary>8000인 교수 찾기
```sql
select name
from instructor
where dept_name ='Comp. Sci.' and salary > 80000
```
## Join절
Comp.Sci dept를 가지고 section.course_id와 course.course_id가 같은 course ID, semester, year, title 찾기
```sql
select section.course_id, semester, year, title
from section, course
where section.course_id = course.course_id and dept_name = 'Comp. Sci.'
```
## natural Join 절
모든 속성에서 같은 이름을 가지고 있는 tuple을 매치해준다.
따라서 불필요한 속성으로 인한 Join을 주의해야 한다.
```sql
select name, course_id
from instructor natural join teaches;
```
## Rename Operations (=as)
## String Operations
%: 어느 substring이던 매치
\_: 어느 character이던 매치

ex) 이름에 dar가 들어가는 교수의 이름 찾기
```sql
select name
from instructor
where name like '%dar%'
```
100%를 매치하려면, like '100 \\%' escape '\\'
## between
ex) 연봉이 90000 ~ 100000인 교수 이름 찾기
```sql
select name
from instructor
where salary between 90000 and 100000
```
## Tuple comparison
```sql
select name, course_id
from instructor, teaches
where (instructor.ID, dept_name) = (teaches.ID,'Biology’);
```
## Set Operations
Set의 작업들은 모두 자동으로 중복을 제거한다.
만약, 중복을 제거하고 싶다면, union all 같이 all을 붙여 사용한다.
ex)
```sql
(select course_id from section where sem = ‘Fall’ and year = 2009)
union
(select course_id from section where sem = ‘Spring’ and year = 2010)
```
### union
합집합
### intersect
교집합
### except
차집합
## Null
null은 어떤 수학적 연산을 해도 null이라는 결과가 나온다.
### unknown
null은 어떤 비교 연산을 해도 unknown이라는 결과가 나온다.
unknown은 null과 비슷하게 `is unknown`이라는 연산을 사용한다.
```sql
OR: (unknown or true) = true,
	(unknown or false) = unknown
	(unknown or unknown) = unknown
AND: (true and unknown) = unknown,
	(false and unknown) = false,
	(unknown and unknown) = unknown
NOT: (not unknown) = unknown
```
## Aggregate Functions
- `avg`: average value
- `min`: minimum value
- `max`: maximum value
- `sum`: sum of values
- `count`: number of values 
	- select count(\*)
### Group By
각 부서의 평균 월급 찾기
```sql
select dept_name, avg (salary) as avg_salary
from instructor
group by dept_name;
```
2010년 봄 학기에 강의를 가르치는 강사의 총 수 찾기
```sql
select count (distinct ID)
from teaches
where semester = 'Spring' and year = 2010;
```

Aggregate Function 외부의 select 절의 속성은 목록별로 그룹화해야 한다.
```sql
select dept_name, ID, avg (salary) # ID는 그룹화 되지 않은 속성이므로 에러가 발생한다.
from instructor
group by dept_name;
```

![[Pasted image 20250320162723.png|300]]
### Having
group by로 가져올 때의 조건을 설정하는 절

부서의 평균 월급이 42000을 넘는 부서의 이름과 평균 월급 찾기
```sql
select dept_name, avg (salary)
from instructor
group by dept_name
having avg (salary) > 42000;
```
Having 절의 술어는 그룹 형성 후에 적용되는 반면 where 절의 술어는 그룹을 형성하기 전에 적용된다.

sum()시에 non-null amount가 없으면 null값을 return한다.
count(\*)을 제외한 모든 aggregate operation은 null을 무시한다.
collection이 비어있다면, count를 제외한 모든 aggregates는 null을 return한다.
## Nested Subqueries
from, where절에서 사용가능하다.

2009년 가을과 2010년 봄에 제공된 강의 찾기
```sql
select distinct course_id
from section
where semester = ’Fall’ and year= 2009 and
	course_id in (select course_id
		from section
		where semester = ’Spring’ and year= 2010);
```

```sql
select course_id
from section as S
where semester = ’Fall’ and year= 2009 and
	exists (select *
		from section as T
		where semester = ’Spring’ and year= 2010
			and S.course_id= T.course_id);
```

2009년 가을에 제공되고, 2010년 봄에 제공되지 않은 강의 찾기
```sql
select distinct course_id
from section
where semester = ’Fall’ and year= 2009 and
	course_id not in (select course_id
		from section
		where semester = ’Spring’ and year= 2010);
```

ID 10101을 가진 강사가 가르친 과정 섹션을 수강한 학생을 중복을 제외한 총 수 찾기
```sql
select count (distinct ID)
from takes
where (course_id, sec_id, semester, year) in
	(select course_id, sec_id, semester, year
	from teaches
	where teaches.ID= 10101);
```

## Set 비교

### some
생물학 부서의 일부 (적어도 한 명) 강사의 급여보다 더 큰 급여를 받는 강사의 이름을 찾으십시오.
```sql
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept name = 'Biology';
```

```sql
select name
from instructor
where salary > some (select salary
					from instructor
					where dept name = 'Biology');
```
some: 서브쿼리에서 반환된 값 중 하나 이상과 비교 조건을 만족하면 TRUE가 된다. ([[ANY]]와 같은 역할)
![[Pasted image 20250320172741.png|200]]
### all
급여가 생물학과의 모든 강사의 급여보다 큰 모든 강사의 이름을 찾기
```sql
select name
from instructor
where salary > all (select salary
	from instructor
	where dept name = ’Biology’);
```
![[Pasted image 20250320172756.png|200]]
### exists, not exists
생물학과에서 제공되는 모든 과목을 수강한 모든 학생들을 찾기
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

### unique
subquery에 중복된 결과가 있는지 테스트

2009년에 최대 한 번 제공되었던 모든 과정 찾기 (두 번이면 안 됨)
```sql
select T.course_id
from course as T
where unique (select R.course_id
		from section as R
		where T.course_id= R.course_id
			and R.year = 2009);
```

평균 급여가 42,000달러 이상인 부서의 평균 강사 급여를 찾기
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

### lateral
서브 쿼리 실행시 각 행별로 동적으로 평가되는 서브쿼리를 사용할 수 있도록 하는 키워드
```sql
select name, salary, avg_salary
from instructor I1, lateral (select avg(salary) as avg_salary
	from instructor I2
	where I2.dept_name= I1.dept_name);
```

### with
with 절이 발생하는 쿼리에 대해서만 정의를 사용할 수 있는 temporary view를 정의하는 방법을 제공

모든 부서 중 최대 예산을 갖는 부서의 최대 예산 값 찾기
```sql
with max_budget (value) as
	(select max(budget)
	from department)
select budget
from department, max_budget
where department.budget = max_budget.value;
```

모든 부서의 총 급여가 총 급여의 평균보다 큰 모든 부서를 찾기
```sql
with dept _total (dept_name, value) as
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
### scalar subquery
```sql
select dept_name,
	(select count(*)
	from instructor
	where department.dept_name = instructor.dept_name)
	as num_instructors
from department;
```
## DDL - 삭제
모든 교수 삭제
```sql
delete from instructor
```

Finance 부서의 모든 교수 삭제
```sql
delete from instructor
where dept_name= 'Finance';
```

Watson 건물에 위치한 부서와 관련된 강사에 대한 강사 관계의 모든 튜플을 삭제
```sql
delete from instructor
where dept_name in (select dept_name
		from department
		where building = 'Watson');
```

급여가 강사의 평균 급여보다 적은 모든 강사를 삭제
```sql
delete from instructor
where salary< (select avg (salary) from instructor);
```

문제: 예금에서 튜플을 삭제하면 평균 급여가 변경됩니다 
SQL에서 사용된 솔루션: 
1. 먼저, 평균 급여를 계산하고 삭제할 모든 튜플을 찾습니다.
2. 다음으로, 위에서 찾은 모든 튜플을 삭제하세요 (avg를 다시 계산하거나 튜플을 다시 테스트하지 않고)

## DDL - Insertion
select, from, where절은 insertion 이전에 계산된다.
아니면 아래 절 같은 곳에서 에러남
ex) `insert into table1 select * from table1`

Add a new tuple to course
```sql
insert into course
values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
```
or equivalently
```sql
insert into course (course_id, title, dept_name, credits)
values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
```
Add a new tuple to student with tot_creds set to null
```sql
insert into student
values ('3003', 'Green', 'Finance', null);
```

tot_creds를 0으로 설정한 학생 관계에 모든 강사 추가
```sql
insert into student
select ID, name, dept_name, 0
from instructor
```

## DDL - Update
급여가 $100,000 이상인 강사의 급여를 3% 인상하고, 다른 모든 강사는 5% 인상을 받습니다.
```sql
update instructor
set salary = salary * 1.03
where salary > 100000;
---
update instructor
set salary = salary * 1.05
where salary <= 100000;
```
### case
```sql
update instructor
set salary = case
	when salary <= 100000 then salary * 1.05
	else salary * 1.03
	end
```
