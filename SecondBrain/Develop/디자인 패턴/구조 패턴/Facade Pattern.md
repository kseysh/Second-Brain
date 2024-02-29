	Facade Pattern?
	복잡한 서브 클래스들의 공통적인 기능을 정의하는 상위 수준의 인터페이스를 제공하는 패턴

여러 SubSystem들의 기능을 하나의 Facade Object로 정의하고, Client가 Facade Object를 사용하는 형태

![[Pasted image 20240229171901.png]]

## 예시

### 기존의 코드 (Facade Pattern 미적용)

#### Washing

```java
class Rinsing{
    void rinse(){
        System.out.println("do Rinsing")
    }
}
```

#### Rinsing

```java
class Spinning{
    void spin(){
        System.out.println("do Spinning")
    }
}
```

#### Spinning

```java
class Washing{
    void wash(){
        System.out.println("do Washing")
    }
}
```

#### Client (sub class 사용 객체)

```java
class Client{
	Washing washing = new Washing();
    Rinsing rinsing = new Rinsing();
    Spinning spinning = new Spinning();
    
    washing.wash();
    rinsing.rinse();
    spinning.spin();
}
```

> 읽기가 힘든 코드이며, 유지보수에 용이하지 않다. Client가 여러 객체들에 `강하게 결합`되어 있다.

---

  

### 📒 Facade Pattern 적용

#### WashingMachine

```java
class WashingMachine{

    Washing washing = new Washing();
    Rinsing rinsing = new Rinsing();
    Spinning spinning = new Spinning();

	void startWashing(){
    	washing.wash();
        rinsing.rinse();
        spinning.spin();
    }
}
```

#### Client

```java
class Client{
    WashingMachine washingMachine = new WashingMachine();
    washingMachine.strartWahsing();
}
```

> Client에서는 Facade Object(WashingMachine)만을 호출하여 '세탁'이라는 동작을 수행할 수 있으며, 메서드의 의미 또한 명확하게 알 수 있다


# Facade Pattern의 장단점

## 장점

- `낮은 결합도`  
    Client가 서브 시스템(SubSystem)들의 코드를 몰라도 된다. Facade Object만 알면 사용이 가능하다. 또한 서브 시스템들간의 복잡한 결합도 역시 낮출 수 있다.
    
- `가독성 상승`  
    기존에는 Client에서 여러 서브 클래스들을 직접 호출해야 했다.  
    하지만 Facade Pattern을 사용하면 하나의 객체만을 호출하여 사용할 수 있고, 그 객체의 네이밍 역시 간단명료할 수밖에 없다.
    
- `서브 시스템 직접 접근 가능`  
    필요에 따라 서브 클래스를 직접 사용할 수도 있다. 선택지가 많아짐.

## 단점
퍼사드가 너무 많은 책임을 가지게 되면, 퍼사드 자체가 복잡해지는 문제가 발생할 수 있다. 또한, 시스템의 세부 기능을 제공하지 못하므로, 복잡한 작업을 수행하려는 클라이언트에게는 제한적일 수 있다,


