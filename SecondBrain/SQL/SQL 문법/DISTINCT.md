범주를 확인할 때 사용
EX) 
테이블에 카테고리라는 컬럼이 존재하고
이 카테고리 값이 테이블에 몇 종류가 있는지 알고 싶을 때,
카테고리를 조회할 때 이 값이 중복되면 안되기 때문에 DISTINCT를 사용한다.

## 사용법
### 컬럼 범주 조회
`SELECT DISTINCT 컬럼 FROM 테이블;`
### 조건 처리 후에 컬럼 범주 조회
`SELECT DISTINCT 컬럼 FROM 테이블 WHERE 조건식;`
### 컬럼 범주 개수 조회
`SELECT COUNT(DISTINCT 컬럼) FROM 테이블;`

