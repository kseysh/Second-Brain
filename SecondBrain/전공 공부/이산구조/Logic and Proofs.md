# Propositional Logic (명제 로직)
## 명제란?
객관적으로 T/F로 답할 수 있다면 명제

## 배타적 논리합
`p⊕q`
p, q 중 하나만 True일 때  True

## p → q 표현하는 방법
`if p, then q`
`if p, q`
`p only if q`
`q when p`
`q unless ¬p`

## 쌍방조건명제
`p ↔ q`
두 값이 같은 경우에만 T

## Truth Table
목표로 하는 복합 명제는 항상 마지막에 들어가도록 한다.

## 흡수 법칙
`p ∨ ( p ∧ q ) ≡ p ∧ ( p ∨ q ) ≡ p`

## 중요한 로직
`p → q ≡ ¬ p v q`
`p ↔ q ≡ ( p → q ) ∧ ( q → p ) ≡ ( p ∧ q ) ∨ ( ¬ p ∧ ¬ q )` 


# Predicate Logic

변수 : x, y, z
명제 : P(x), M(x)
한정자 ∀, ∃
이 세 특징을 가진다.

> R(x, 3, z)처럼 변수가 들어간 표현은 명제가 아니며, 따라서 truth value를 갖지 않는다.

## 영어를 Logic으로 바꾸기
Example 많이 풀어보기

## 한정자에 따른 명제

∀는 `∀x(S(x)→J(x))`
ex ) Every student in this class has taken a course in Java
∃는 `∃x(S(x)∧J(x))`
ex ) Some student in this class has taken a course in Java

## 한정자의 부정
¬∀xS(x) ≡ ¬∀xS(x)