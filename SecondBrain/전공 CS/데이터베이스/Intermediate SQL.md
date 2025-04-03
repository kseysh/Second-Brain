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
한 번 뷰가 정의되면, 뷰 이름을 통해 