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
## equi join
- join condition에서 =을 사용하는 join
## cross join
- 두 table의 tuple pair로 만들 수 있는 모든 조합을 result table로 반환한다
- join condition이 없다.
- implicit cross join: FROM table1, table2 
- explicit cross join: FROM table1, CROSS JOIN table2
 - MySQL에서는 cross join = inner join = join이다
 - CROSS JOIN에 ON(or USING)을 같이 쓰면 inner join으로 동작한다
 - INNER JOIN(or JOIN)이 ON(or USING) 없이 사용되면 cross join으로 동작한다.