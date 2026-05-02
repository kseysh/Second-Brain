A## with절

> 가상 테이블을 만드는 함수
> 메인쿼리에서 쓸 서브쿼리를 미리 with절에 기술해주는 것

```
WITH 가상테이블명 AS 
(
	SELECT 쿼리 
	FROM 테이블 이름
	WHERE 조건
)
```

### 예제
부서 이름이 'Sales'인 모든 직원의 이름, 직책 및 급여 정보를 가져오는 쿼리
```
WITH SalesEmployees AS ( 
	SELECT Name, JobTitle, Salary 
	FROM employees e 
	JOIN departments d 
	ON e.DepartmentID = d.DepartmentID 
	WHERE DepartmentName = 'Sales' 
) 
SELECT Name, JobTitle, Salary 
FROM SalesEmployees;
```

with절을 사용하지 않을 때
```
SELECT e.Name, e.JobTitle, e.Salary 
FROM employees e 
JOIN departments d 
ON e.DepartmentID = d.DepartmentID 
WHERE d.DepartmentName = 'Sales';
```
이 예제에서는 중복된 쿼리가 많지 않아 with절을 사용하지 않았을 때가 쿼리의 가독성을 더 높일 수 있지만, with절을 사용했던 부분이 중복된다면 with절의 사용을 고려해보는 것이 좋다.
### 장점
하나 이상의 서브쿼리에서 반환된 데이터를 단일 쿼리에서 재사용할 수 있다.
코드 중복을 줄이고 쿼리의 가독성을 향상시키는 데 도움이 된다.

temp라는 임시 테이블을 사용해서 **장시간 걸리는 쿼리의 결과를 저장해놓고 저장해놓은 데이터를 엑세스**하기 때문에 성능이 좋다
> 하지만 with절을 너무 많이 사용하여 같은 시간에 여러 개의 with절을 동시에 돌리면 임시테이블이 견딜 수 있는 정도를 넘어가 다같이 느려진다는 단점이 있다.
### 단점
가상의 테이블을 생성하는 쿼리로, 메모리를 차지한다는 단점이 있다.

## WITH RECURSIVE (재귀 쿼리)
자기 자신을 참조하여 루프를 도는 방식

```sql
WITH RECURSIVE Calendar AS (
    SELECT '2022-01-01' AS DT -- Start Date
    UNION ALL
    SELECT DATE_ADD(DT, INTERVAL 1 DAY) 
    FROM Calendar 
    WHERE DT < '2022-01-31' -- End Condition
)
SELECT * FROM Calendar;
```
특정 달의 1일 부터 말일까지의 날짜를 생성해야 할 때 유용

## 주의할 점
여러 개를 정의할 때는 쉼표로 연결한다.
```sql
WITH ROOT_IDS AS (
    SELECT ID FROM ECOLI_DATA WHERE PARENT_ID IS NULL
), -- 쉼표로 연결
SECOND_IDS AS (
    SELECT ID FROM ECOLI_DATA 
    WHERE PARENT_ID IN (SELECT ID FROM ROOT_IDS)
) -- 여기서 WITH를 또 쓰지 않음
SELECT ID
FROM ECOLI_DATA
WHERE PARENT_ID IN (SELECT ID FROM SECOND_IDS)
ORDER BY ID;
```