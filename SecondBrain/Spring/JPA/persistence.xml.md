## 필수 속성 정리
데이터베이스에 대한 접근 정보를 넣기 위함이다.

`<property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>  `
: 어떤 데이터베이스 드라이버를 사용할 것인지
`<property name="javax.persistence.jdbc.user" value="sa"/>  `
: 데이터베이스 사용자 이름
`<property name="javax.persistence.jdbc.password" value=""/>  `
: 데이터베이스 사용자 비밀번호
`<property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>  `
: 데이터베이스 접근 url
`<property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>`
: [[데이터베이스 방언]]을 jpa가 번역할 수 있도록 해줌