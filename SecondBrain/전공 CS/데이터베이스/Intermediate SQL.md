## Outer Join
left outer join
right outer join
full outer join
### example
![[Pasted image 20250403141345.png|200]]

`course natural left outer join prereq`
![[Pasted image 20250403141449.png|300]]
cs-315는 prereq에 없으므로 null로 표현된다

`course natural right outer join prereq`
![[Pasted image 20250403141525.png|300]]
cs-347은 course에 없으므로 null로 표현된다.

`course natural full outer join prereq`
![[Pasted image 20250403141641.png|300]]

`course inner join prereq on course.course_id = prereq.course_id`
![[Pasted image 20250403141738.png|300]]

`course left outer join prereq on course.course_id = prereq.course_id`
![[Pasted image 20250403141751.png|300]]

`course full outer join prereq using (course_id)`
![[Pasted image 20250403141854.png|300]]
## Views
모든 사용자가 전체 논리 모델을 보는 것이 바람직하지 않은 경우에 사용
특정 사용자에게 특정 데이터를 숨기기 위한 메커니즘 제공
개념적 모델에는 없지만 사용자에게 가상 릴레이션으로 보이는 릴레이션
```sql
create view v as <쿼리 표현식>
```
<쿼리 표현식>은 어떤 SQL 표현식이든 가능하며, v는 뷰의 이름을 나타냄
한 번 뷰가 정의되면, 뷰 이름을 통해 뷰가 생성하는 가상 relation을 참조할 수 있음
view 정의는 쿼리 결과를 미리 계산해 새 relation을 만드는 것이 아닌, 쿼리 표현식만을 저장하는것

#### example
급여 정보 없이 강사 정보를 보여주는 뷰
```sql
create view faculty as  
select ID, name, dept_name  
from instructor;
```
생물학과 소속 강사들의 이름 조회
```sql
select name  
from faculty  
where dept_name = 'Biology';
```
학과별 총 급여를 보여주는 뷰
```sql
create view departments_total_salary(dept_name, total_salary) as  
select dept_name, sum(salary)  
from instructor  
group by dept_name;
```
다른 뷰를 사용해 정의된 뷰
```sql
create view physics_fall_2009 as  
select course.course_id, sec_id, building, room_number  
from course, section  
where course.course_id = section.course_id  
and course.dept_name = 'Physics'  
and section.semester = 'Fall'  
and section.year = 2009;

create view physics_fall_2009_watson as  
select course_id, room_number  
from physics_fall_2009  
where building = 'Watson';
```
## View update
```sql
insert into faculty values ('30765', 'Green', 'Music');
```
이는 실제로 아래 튜플을 instructor relation에 삽입하는 것으로 해석되어야 함
```sql
('30765', 'Green', 'Music', null)
```
salary값은 명시되지 않았으므로 null로 저장됨
### 고유하게 변환할 수 없는 Update
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
Taylor 건물에 여러 개의 학과가 있다며느 어떤 학과로 지정해야 할지 모르며,
Taylor 건물에 어떤 학과도 없다면 삽입이 불가능한 문제가 발생한다.
### 일반적으로 SQL 뷰가 updatable하다고 판단되는 경우
- from 절에 하나의 relation만 포함되어 있을 때
- select절에 relation의 속성 이름만 포함되어 있고, 표현식, 집계 함수, distinct등이 포함되어 있지 않을 때
- select 절에 포함되지 않은 속성이 nullable할 때
- group by나 having 절이 쿼리에 포함되어 있지 않을 때
#### 위 조건을 만족해도, 갱신이 불가능한 경우
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
where절의 조건 (즉, dept_name = 'History)을 만족하지 않기 때문에 insert가 차단된다.
with check option 절이 