>CASE 문은 조건을 통과하고 첫 번째 조건이 충족되면 값을 반환한다.
  조건에 따라 True(참)이면 읽기를 중지하고 결과를 반환하고, 조건이 True(참)가 아니면 ELSE 절의 값을 반환한다.
  ELSE 부분이 없고 조건이 참이 아니면 NULL을 반환합니다.

```
# CASE문 사용 방법 
CASE 
	WHEN 조건1 THEN 결과값1 
	WHEN 조건2 THEN 결과값2 
	WHEN 조건N THEN 결과값N 
	ELSE 결과값 
END
```

>TIP
>위의 예시처럼 사용하면 컬럼이름이 길어지므로 아래처럼 바꿔주면 좋다.

```
# CASE Query 
SELECT 
	id AS id, 
	(
	CASE 
		WHEN number = 1 THEN 'fruit' 
		WHEN number = 2 THEN 'vegetable' 
		WHEN number = 3 THEN 'animal' 
		ELSE 'not' 
	END 
	) AS type, 
	kind AS type_desc 
FROM 
	test;
```
