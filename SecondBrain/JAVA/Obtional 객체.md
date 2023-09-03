# NullPointerException이란?
개발을 하며 자주 발생하는 예외 중 하나이며, Null 참조를 사용하는 경우 발생하는 에러이다. NPE를 피하려면 Null 여부를 검사해야 하는데, Null 검사를 해야하는 변수가 많은 경우 코드가 복잡해져서 Null 대신 초기값을 사용하는 것을 권장하기도 한다..

# Optional이란?
Java8에서는 Optional\<T>클래스를 사용해 NPE를 방지할 수 있도록 도와준다. Optional\<T>는 Null이 올 수 있는 값을 감싸는 Wrapper 클래스로 참조하더라도 NPE가 발생하지 않도록 도와준다. (따라서 Optional 인스턴스는 모든 타입의 참조 변수를 저장할 수 있다.)

# Optional 객체의 생성
of() 메소드나 ofNullable() 메소드를 사용하여 Optional 객체를 생성할 수 있습니다.
of() 메소드는 null이 아닌 명시된 값을 가지는 Optional 객체를 반환합니다.
만약 of() 메소드를 통해 생성된 Optional 객체에 null이 저장되면 NullPointerException 예외가 발생합니다.
따라서 만약 참조 변수의 값이 만에 하나 null이 될 가능성이 있다면, ofNullable() 메소드를 사용하여 Optional 객체를 생성하는 것이 좋습니다.