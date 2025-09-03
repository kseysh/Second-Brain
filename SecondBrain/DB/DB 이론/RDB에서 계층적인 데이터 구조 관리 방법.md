- Adjacency list
- Nested set
- Path Enumeration
- Closure table

> 계층적인 데이터 구조를 관계형 데이터베이스에 저장할 때는, 서버가 데이터베이스의 무결성을 책임져야 한다.
## Adjacency list
관계형 데이터베이스에 계층적인 데이터 구조를 저장할 때, 테이블의 컬럼 하나를 부모 레코드의 기본키로 가지도록 하는 전략
- 단순한 전략이라 적용이 쉽지만, 재귀 쿼리를 사용해야 하기에 성능면에서 비효율적이다.
### 장점
단순한 전략이라 적용이 쉽ㄷ
### 단점
## Nested Set
각 데이터에 left와 right 두 개의 숫자를 부여하여 트리 구조를 표현하는 방식 모든 하위 댓글은 부모 댓글의 left와 right 값 사이에 위치한다.
![[Pasted image 20250903160650.png|300]]
위 그림과 맞는 예제 데이터
```sql
-- 부모
INSERT INTO family (id, role, lft, rgt) VALUES (NULL, '부모', 1, 18);

-- 부모 > 딸
INSERT INTO family (id, role, lft, rgt) VALUES (NULL, '딸', 2, 11);

-- 부모 > 아들
INSERT INTO family (id, role, lft, rgt) VALUES (NULL, '아들', 12, 17);

-- 부모 > 딸 > 자녀
INSERT INTO family (id, role, lft, rgt) VALUES (NULL, '아들', 3, 4);
INSERT INTO family (id, role, lft, rgt) VALUES (NULL, '아들', 5, 6);
INSERT INTO family (id, role, lft, rgt) VALUES (NULL, '아들', 7, 8);
INSERT INTO family (id, role, lft, rgt) VALUES (NULL, '딸', 9, 10);

-- 부모 > 아들 > 자녀
INSERT INTO family (id, role, lft, rgt) VALUES (NULL, 'daughter', 13, 14);
INSERT INTO family (id, role, lft, rgt) VALUES (NULL, 'son', 15, 16);
```

특정 계층 내에 있는 데이터를 검색하고 싶다면, left field와 right field를 활용해 아래와 같은 SQL 구문을 사용한다.
```sql
SELECT * FROM family WHERE lft BETWEEN 2 AND 11;
```
=> 데이터 추가/삭제가 어려워 무결성을 유지하기 어려운 전략이다.
또한, 새로운 데이터를 추가하거나 기존 데이터를 삭제할 때마다 해당 데이터 이후의 모든 노드의 left와 right 값을 조정해야하는 상황이 생길 수 있어 부하가 크다.
## Closure table
다대다 관계를 일대다 다대일의 관계로 풀듯이, 계층간의 관계를 다른 테이블에서 관리하는 전략을 의미한다.
```sql
CREATE TABLE family (
  id SMALLINT(5) NOT NULL AUTO_INCREMENT,
  role VARCHAR(50) NOT NULL,
  PRIMARY KEY(id)
);

CREATE TABLE family_relation (
  ancestorId SMALLINT(5) NOT NULL,
  descendantId SMALLINT(5) NOT NULL,
  depth SMALLINT(5) NOT NULL,
  PRIMARY KEY(ancestorId, descendantId)
);
```
무결성을 검증하기 쉬운 전략이다.

하지만, A > B > C일 경우 (A, B)와 (B,C)만 저장한다면, A에서 모든 자손을 찾을 때, Adjacency list와 같은 재귀 탐색으로 인한 비효율이 발생한다.
따라서 A > B일 경우에서 A > B > C로 C를 추가해야 한다면, (A, C) (B, C)도 저장해야 한다.
이는 특정 노드의 모든 자손은 한 번의 쿼리로 빠르게 찾을 수 있다는 장점과 삽입, 삭제에서 비효율적인 모습을 보이는 단점을 뚜렷하게 관찰할 수 있다.
## Path Enumeration
일반적으로 아래와 같은 문자열의 형태로 계층적 데이터를 저장한다.
`1/2/6/7/8`
