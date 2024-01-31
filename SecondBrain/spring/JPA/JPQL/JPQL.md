# JPQL(Java Persistence Query Language)

>자바에서 관계형 데이터베이스와 상호 작용하기 위한 객체 지향 쿼리 언어

JPQL은 JPA(Java Persistence API)의 일부로 사용되며, 데이터베이스에서 데이터를 조회, 수정, 삭제하기 위한 표준화된 방법을 제공

JPQL은 SQL(Structured Query Language)과 비슷한 문법을 가지고 있지만, 데이터베이스 테이블이 아닌 엔티티 객체와 필드를 대상으로 쿼리를 작성한다. JPQL을 사용하면 객체 지향적인 방식으로 데이터를 검색하고 조작할 수 있으며, 데이터베이스 종속성을 최소화할 수 있다.

- 가장 단순한 조회 방법
- EntityManager.find()
- 객체 그래프 탐색(a.getB(),getC())

JPQL의 주요 특징

1. **객체 지향적인 쿼리:** JPQL은 [[엔티티]]와 관련된 쿼리를 객체 지향적으로 작성할 수 있습니다. 엔티티 클래스와 필드를 사용하여 쿼리를 작성하므로 데이터베이스 구조와 직접적으로 연결되지 않습니다.
    
2. **포함된 엔티티와 관계:** JPQL은 엔티티 간의 관계를 사용하여 쿼리를 작성할 수 있습니다. JOIN을 사용하여 연관된 엔티티를 검색하거나 연관된 엔티티의 속성을 참조할 수 있습니다.
    
3. **타입 안전성:** JPQL은 타입 안전성을 제공하므로 컴파일 시에 쿼리 오류를 확인할 수 있습니다. 이는 오타나 잘못된 엔티티 속성에 대한 오류를 사전에 방지할 수 있습니다.
    
4. **매개변수 바인딩:** JPQL에서는 매개변수를 사용하여 동적 쿼리를 작성할 수 있습니다. 이를 통해 쿼리에 런타임 값들을 바인딩할 수 있습니다.
    
5. **쿼리 결과:** JPQL은 SELECT 쿼리를 실행하고 엔티티 객체나 스칼라 값(단일 값)을 반환할 수 있습니다. 결과는 엔티티 객체 컬렉션 또는 단일 값이 될 수 있습니다.
> JPQL은 엔티티 객체를 대상으로 쿼리문을 실행하지만, SQL은 데이터베이스 테이블을 대상으로 쿼리문을 실행한다.

## JPQL 문법 특징
엔티티와 속성은 대소문자를 구분한다.
JPQL 키워드는 대소문자를 구분하지 않는다 (SQL문처럼)
엔티티 이름을 사용하며, 테이블 이름을 사용하지 않는다.
별칭을 필수로 만들어 주어야 한다
ex) `select m from Member m where m.age > 18`
에서의 m을 별칭이라고 한다.

## 파라미터 바인딩
ex)
```
SELECT m FROM Member m where m.username=:username
query.setParameter("username",usernameParam);
```

## 프로젝션
SELECT 절에 조회할 대상을 지정하는 것
프로젝션 대상: 엔티티, 임베디드 타입, 스칼라 타입(기본 데이터 타입)

`select m from member m` => 엔티티 프로젝션
`select m.team from member m` => 엔티티 프로젝션
`select m.address from member m` => 임베디드 타입 프로젝션
`select m.username, m.age from member m` => 스칼라 타입 프로젝션

### 프로젝션 - 여러 값 조회
- Query 타입으로 조회
- new 명령어로 조회
	- 단순값을 DTO로 바로 조회
	  ex)
	  `select new jpabook.jpql.UserDTO(m.username, m.age) from Member m`
	  패키지 명을 포함한 전체 클래스 명을 입력해야 한다.
	  순서와 타입이 일치하는 생성자가 필요하다.
