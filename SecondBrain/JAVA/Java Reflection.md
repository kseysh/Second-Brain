Runtime에 클래스의 메타데이터를 분석하고 객체의 필드/메서드/생성자에 동적으로 접근,호출할 수 있게 해주는 기능

이로 인해 컴파일 시점에 타입을 몰라도 런타임에 구조를 다룰 수 있음

## 구성 요소
#### Class\<?>
- 클래스 메타데이터의 진입점
- 클래스는 로딩 시 단 하나의 Class를 가짐
ex) `Class<Member> clazz = Member.class;`
#### Field 조회
- private 필드 접근 가능
- 캡슐화 우회 가능
ex)
```java
Field field = clazz.getDeclaredField("name");
field.setAccessible(true);
Object value = field.get(member);
```
#### 메서드 호출
ex)
```java
Method method = clazz.getDeclaredMethod("changeName", String.class);
method.setAccessible(true);
method.invoke(member, "kim");
```
#### 생성자 호출
```java
Constructor<Member> ctor = clazz.getDeclaredConstructor(String.class);
Member member = ctor.newInstance("kim");
```
#### 어노테이션 확인
```java
if (clazz.isAnnotationPresent(Entity.class)) {
    // JPA 엔티티 처리
}
```
## 사용 사례
- Spring
    - 컴포넌트 스캔
    - DI
    - AOP 프록시
- JPA / Hibernate
    - `@Entity`, `@Column`, `@Id`
- JacksonN 직렬화 / 역직렬화
- 테스트 프레임워크
    - private 메서드 테스트

## 장점과 단점
### 장점
- 높은 유연성
- 런타임 확장성
- 프레임워크 구현에 필수
### 단점
- 성능 오버헤드
- 컴파일 타임 타입 체크 불가
- 캡슐화 파괴
- 코드 가독성 저하
### 제언
- 애플리케이션 코드에서는 최소화    
- 프레임워크 내부에서만 적극 사용