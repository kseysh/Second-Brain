가능한 한 많은 관계를 맺어주어 재사용성을 높이고 관리하기 쉽게 한다.
is-a has-a를 가지고 문장을 만들어 본다. 
두 개의 문장을 만들어보고, 둘 중 어떤 것이 더 자연스러운지 확인한다.

- is-a ( ~는 ~다. )  - **상속관계**
원(Circle)은 점(Point)이다 - Circle **is a** Point
```
class Circle extends Point{
	int r; // 반지름 
}
```
- has-a ( ~는 ~를 가지고 있다. )  - **포함관계**
원(Circle)은 점(Point)을 가지고 있다. - Circle **has a** Point
```
class Circle {
	Point c = new Point(); // 원
	int r; // 반지름 
}
```

만약 객체간 여러 관계를 맺고 싶을 때 Java는 단일상속만을 허용하므로 비중이 높은 클래스 하나만 상속 관계로, 나머지는 포함관계로 한다.


