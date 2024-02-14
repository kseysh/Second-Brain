this() - 생성자, 같은 클래스의 다른 생성자를 호출할 때 사용 다른 생성자 호출은 생성자의 첫 문장에서만 가능

ex)
```
class Car {
	String color;
	String gearType;
	int door;

	Car(){
		this("white", "auto", 4);
	}
	Car(String c, String g, int d){
		this.color = c;
		this.gearType = g;
		this.door = d;
	}
}
```