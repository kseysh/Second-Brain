## NULL과 SQL three-valued logic
- SQL에서 NULL과 비교 연산을 하게 되면 그 결과는 UNKNOWN이다
- UNKNOWN은 'TRUE일 수도 있고 FALSE일 수도 있다'라는 의미이다
### three-valued logic
비교/논리 연산의 결과로 TRUE, FALSE, UNKNOWN을 가진다.
### UNKNOWN이라고 표기하는 이유
SQL에서 NULL의 의미는 
unknown
unavailable or withheld
not applicable
의 세 가지 의미를 가지기 때문에, NULL이 만약 유효한 값이라면 결과가 어떻게 나올지 모르기 때문이다.
따라서 NULL = NULL의 결과도 UNKNOWN이다.

## Where절의 conditions
- where절에 있는 condition의 결과가 TRUE인 tuple만 선택된다
- 즉, 결과가 FALSE거나 UNKNOWN이면 tuple은 선택되지 않는다.
### 관련한 NOT IN 사용 시 주의 사항
![[Pasted image 20250126213311.png|400]]
not in은 하나씩 비교해서 and 연산을 하는 로직인데, 3과 NULL을 비교하며 결과가 UNKNOWN이 되고, UNKNOWN과 TRUE를 and 연산하면서 결과가 UNKNOWN이 되는 것이다.

![[Pasted image 20250126213616.png|400]]
위 서브 쿼리에서 dept_id가 NULL인 employee가 하나라도 있다면, 전체 결과가 UNKNOWN이 되면서 select된 데이터가 하나도 없게 된다.

이를 해결하기 위해 dept_id에 NOT NULL constraints를 걸어두거나, 서브쿼리에 dept_id IS NOT NULL의 조건을 걸어둔다.