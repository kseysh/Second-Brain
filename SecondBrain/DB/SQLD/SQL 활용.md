# GROUP BY FUNCTION
### GROUPING SETS (A,B)
A별 B별 그룹 연산 결과 출력
### ROLLUP (A,B)
A별 (A,B)별 전체 그룹 연산 결과 출력
### CUBE (A,B)
A별 B별 (A,B)별 전체 그룹 연산 결과 출력
# 서브 쿼리 네이밍
### 스칼라 서브 쿼리
select 절에 사용하는 쿼리
### 인라인 뷰
from에서 테이블 별칭 명시하는 것
### 상호 연관 서브 쿼리
메인 쿼리와 서브 쿼리의 비교를 수행하는 형태

# PIVOT
select \* from 테이블 명 또는 서브 쿼리 pivot (**value** for **unstack** in (1,2,3))
## UNPIVOT
VALUE FOR STACK




# 새로 안 사실
rollup 및 cube는 한 개만 전달 시 전체 그룹 연산 결과 출력 x
DISTINCT는 중복된 값을 받지 않는 함수
인라인뷰: from에서 테이블 별칭 정의한 것
group by 안쓰고 having 써도 댐
GROUPING: group by에 의해 산출된 row면 0을 rollup, cube에 의해 산출된 row면 1 반환
top n 질의문에서 n에 해당하는 값이 동일한 경우 함께 출력되도록하는 with ties 옵션을 사용해야함