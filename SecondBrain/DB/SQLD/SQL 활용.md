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







# 새로 안 사실
rollup 및 cube는 한 개만 전달 시 전체 그룹 연산 결과 출력 x
DISTINCT는 중복된 값을 받지 않는 함수
인라인뷰: from에서 테이블 별칭 정의한 것
group by 안쓰고 having 써도 댐
GROUPING: group by에 의해 산출된 row면 0을 rollup, cube에 의해 산출된 row면 1 반환