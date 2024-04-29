## HashSet
**Set 인터페이스를 구현한 대표적인 컬렉션 클래스**
순서를 유지하려면, LinkedHashSet 클래스를 사용하면 된다.

객체를 저장하기 전에 기존에 같은 객체가 있는지 확인한다.
같은 객체가 없으면 저장하고, 있으면 저장하지 않는다.
boolean add(Object o)는 저장할 객체의 equals()와 hashCode()를 호출한다. 따라서 equals()와 hashCode()가 오버라이딩 되어 있어야 한다.
> hashCode()의 오버라이딩 조건
> 동일 객체에 대해 hashCode()를 여러 번 호출해도 동일한 값을 반환해야 한다.
> equals로 비교해서 true를 얻은 두 객체의 hashCode()값은 일치해야 한다.
> equals()로 비교한 결과가 false인 두 객체의 hashCode()값이 같을 수도 있다. 그러나 성능 향상을 위해 가능하면 서로 다른 값을 반환하도록 작성하라.
## TreeSet
**범위 검색과 정렬에 유리**한 컬렉션 클래스 (이진 검색 트리로 구현)
HashSet보다 데이터 추가, 삭제에 시간이 더 걸림
### TreeSet의 데이터 저장과정
![[Pasted image 20240424231712.png]]

### 주요 메서드
![[Pasted image 20240424231809.png]]
![[Pasted image 20240424231816.png]]
