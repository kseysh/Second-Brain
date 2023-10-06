테이블에 존재하는 데이터에서 최대값, 최소값을 가져오고 싶은 경우 사용
> MAX, MIN은 숫자만이 아닌 문자형 데이터에서도 사용가능하다.

`SELECT MAX(컬럼) FROM 테이블;`

`SELECT MIN(컬럼) FROM 테이블;`
## 예시
예제 테이블 : products
![[Pasted image 20231006194416.png]]
### 가장 높은 가격 가져오기
`SELECT MAX(price) AS max_price FROM products;`
### 결과
![[Pasted image 20231006194452.png]]
