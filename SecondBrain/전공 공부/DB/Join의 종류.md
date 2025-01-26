## implicit join / explicit join
![[Pasted image 20250126214943.png|400]]
이는 JOIN 키워드를 통해 아래처럼 가독성 있는 쿼리로 작성 가능하다
```sql
SELECT D.name
FROM employee AS E JOIN department AS D ON E.dept_id = D.id
WHERE E.idd = 1;
```
이를 explicit join이라 한다
![[Pasted image 20250126215201.png|400]]

![[JOIN]]
### equi join
- join condition