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
### Nested Subqueries

2009년 가을과 2010년 봄에 offer
```sql
select distinct course_id
from section
where semester = ’Fall’ and year= 2009 and
	course_id in (select course_id
		from section
		where semester = ’Spring’ and year= 2010);
```
not in을 사용할 수도 있다.
