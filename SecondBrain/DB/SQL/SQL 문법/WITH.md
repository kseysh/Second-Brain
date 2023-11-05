## with절

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

### 장점
하나 이상의 서브쿼리에서 반환된 데이터를 단일 쿼리에서 재사용할 수 있다.
코드 중복을 줄이고 쿼리의 가독성을 향상시키는 데 도움이 된다.
