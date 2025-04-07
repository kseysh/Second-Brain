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
###### Selecting attributes ID and salary from the instructor relatijon Relational Algebra로
∏<sub>ID,salary</sub>(instructor)
###### Natural Join을 다르게 표현하면
⋈ = ∏<sub>조건</sub>(𝜎<sub>조건</sub>(r X s))
###### Theta Join을 다르게 표현
![[Pasted image 20250318142719.png|300]]
###### Theta join 특징
Theta join자체에는 Projection(select)를 진행하지 않음
###### Union 특징
중복 제거
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
where = 그룹 형성 후 적용
###### Aggregation Function과 Null
SUM() 함수는 집계 대상에 모든 값이 NULL일 경우, NULL을 반환
COUNT(\*)를 제외한 대부분의 집계 함수는 NULL 값을 무시하며,
집계 대상이 비어 있을 경우(조건에 해당하는 행이 없을 경우),
COUNT는 0을 반환하고, 나머지 집계 함수들은 NULL을 반환한다.
###### 2009년 가을과 2010년 봄에 제공된 강의 찾기 nested subquery를 이용해서
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
###### 2009년 가을에 제공되고, 2010년 봄에 제공되지 않은 강의 찾기
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
where (course_id, sec_id, semester, year) in
	(select course_id, sec_id, semester, year
	from teaches
	where teaches.ID= 10101);
```
###### some
서브쿼리에서 반환된 값 중 하나 이상과 비교 조건을 만족하면 TRUE ([[ANY]]와 같은 역할)
###### all
![[Pasted image 20250320172756.png|200]]
###### exists, not exists

###### unique

###### with


######

###### 생물학 부서의 일부 (적어도 한 명) 강사의 급여보다 더 큰 급여를 받는 강사의 이름을 찾기 (Set 비교 활용)
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
