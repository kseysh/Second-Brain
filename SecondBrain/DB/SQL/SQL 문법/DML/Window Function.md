
## Window Function
GROUP BY와 달리 행의 수를 줄이지 않으면서 집계 데이터를 각 행에 붙여줄 때 사용

## RANK() vs DENSE_RANK() vs ROW_NUMBER()
- `RANK()`: 1, 2, 2, 4 (공동 순위만큼 건너뜀)
- `DENSE_RANK()`: 1, 2, 2, 3 (공동 순위가 있어도 연속됨)
- `ROW_NUMBER()`: 1, 2, 3, 4 (무조건 고유 번호 부여)

## ROW_NUMBER()
각 PARTITION 내에서 ORDER BY절에 의해 정렬된 순서를 기준으로 고유한 값을 반환하는 함수
그룹 내 순위 함수
### 문법
`ROW_NUMBER() OVER(PARTITION BY [그룹핑할 컬럼] ORDER BY [정렬할 컬럼])`
- PARTITION BY는 선택, ORDER BY는 필수
## DENSE_RANK
동일한 값이면 중복 순위를 부여하고, 다음 순위는 중복 순위와 상관없이 순차적으로 반환한다
1,2,2,3 등으로 순위를 매긴다.