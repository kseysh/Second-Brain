임시 테이블을 다룰 때 사용한다.

MySQL에서 임시테이블(Temporary table)은 단일 세션에서 여러 번 재사용할 수 있는 임시 결과 세트를 저장할 수 있는 특수한 유형의 테이블이다.

임시 테이블은 반복적인 SELECT 쿼리를 해야 하거나, 대량의 집계 쿼리와 같이 비용이 클 때 유용하게 활용할 수 있다.