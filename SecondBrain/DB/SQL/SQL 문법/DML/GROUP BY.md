관심있는 attributes를 기준으로 그룹을 나눠서 그룹별로 aggregate function을 적용하고 싶을 때 사용
GROUP BY를 사용할 때는 두가지가 필요하다.
특정 컬럼을 그룹화 하는 **GROUP BY** 
특정 컬럼을 그룹화한 결과에 조건을 거는 [[HAVING]]

> WHERE와 HAVING을 헷갈릴 수 있지만, WHERE는 그룹화 하기 전에 수행하고, HAVING은 그룹화 후의 조건이다.

EX)
### 컬럼 그룹화
`SELECT 컬럼 FROM 테이블 GROUP BY 그룹화할 컬럼;`
### 조건 처리 후에 컬럼 그룹화
`SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼;`
### 컬럼 그룹화 후에 조건 처리
`SELECT 컬럼 FROM 테이블 GROUP BY 그룹화할 컬럼 HAVING 조건식;`
### 조건 처리 후에 컬럼 그룹화 후에 조건 처리
`SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼 HAVING 조건식;`
### ORDER BY가 존재하는 경우
`SELECT 컬럼 FROM 테이블 [WHERE 조건식]`
`GROUP BY 그룹화할 컬럼 [HAVING 조건식] ORDER BY 컬럼1 [, 컬럼2, 컬럼3 ...];`


## GROUP BY 활용

### COUNT(\*) 조합
데이터가 얼만큼 들어가 있는지 확인하는 용도로 사용할 수 있다.
![[Pasted image 20231117185316.png]]
![[Pasted image 20231117185323.png]]

### SUM(\*) 조합
![[Pasted image 20231117185352.png]]
![[Pasted image 20231117185401.png]]

### AVG(\*) 조합
![[Pasted image 20231117185420.png]]
![[Pasted image 20231117185427.png]]