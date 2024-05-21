# DML (Data Manipulation)종류
- SELECT
- INSERT
- UPDATE
- DELETE
- **MERGE**
- 저장(commit) 혹은 취소(rollback) 반드시 필요
# **DCL (Data controll)종류**
- GRANT
- REVOKE
# DDL (Data Definition)종류
- CREATE
- ALTER
- DROP
- **TRUNCATE**
- **RENAME**
# TCL(Transaction Controll) 종류
-  COMMIT
- ROLLBACK

# SQL문 실행순서 (잘 생각해보면 이해감)
- FROM
- WHERE
- GROUP BY
- HAVING
- SELECT
- ORDER BY

# 추가적인 정보
 Oracle에서 길이가 0인 데이터는 Null이다
 replace 뒤에 값 없으면 삭제
 오라클에서 1/24(60/10)은 10분
 =조건만 있는 경우 검색 CASE 표현식을 단순 CASE 표현식으로 변환할 수 있다.
 case문은 DECODE('어떤 값에서','타겟값과 같으면','A로 바꾸고','아니면 B')로 사용가능
ISNULL이나 NVL은 대상이 널이면 치환값으로 치환하여 리턴한다.
SUBSTR(문자열,b,c)은 문자열 중 b위치에서 c개의 문자 길이에 해당하는 문자를 돌려줌 
GROUP BY를 사용할 경우 GROUP BY 표현식이 아닌 값은 GROUP BY 뒤에서 사용될 수 없다
