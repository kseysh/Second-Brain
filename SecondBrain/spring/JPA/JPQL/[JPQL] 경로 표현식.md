## 상태 필드
단순히 값을 저장하기 위한 필드

경로 탐색의 끝으로 탐색하지 않는다.
## 연관 필드
연관관계를 위한 필드
### 단일 값 연관 경로
	(@ManyToOne, @OneToOne) => 대상이 엔티티
묵시적 내부 조인(inner join)이 발생되며, 탐색이 가능하다. (묵시적 내부 조인은 조심해서 사용해야 한다.)
ex) `select m.team.members from Member m` 처럼 탐색
### 컬렉션 값 연관 경로
	  (@OneToMany, @ManyToMany) => 대상이 컬렉션
묵시적 내부 조인이 발생하며 탐색하지 않는다.

*하지만*) FROM 절에서 명시적 조인을 통해 별칭을 얻으면 별칭을 통해 탐색이 가능하다.
ex) 
`select t.members from Team t`는 members에서 더 탐색을 진행할 수 없지만,
`select m from Team t join t.members m`은 m이라는 별칭을 얻어 더 탐색을 진행할 수 있다.
# 결론
묵시적 내부 조인을 최대한 줄이고 명시적 조인을 사용하는 것이 나중에 쿼리 튜닝에 유용하다!

ex)
`select o.member.team from Order o`
=>
`select m.username from Team t join t.members m`
으로 명시적 조인으로 변경하는 것이 좋다.