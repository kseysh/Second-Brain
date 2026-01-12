## 스레드 영역
### 네이티브 메소드 영역
자바 외 언어로 작성된 네이티브 코드를 위한 메모리
### PC 레지스터 영역
현재 실행중인 JVM 주소 저장
### 스택 영역
  메소드 호출 시 생성되는 스레드 수행정보 기록
## 공유 영역
### 힙 영역
런타임시 동적으로 할당하여 사용하는 메모리 영역
### 메서드 영역 (Metaspace Area (Java 8 ~) / Class Area, Static Area)
JVM이 시작될 때 생성되는 공간으로 바이트 코드를 처음 메모리 공간에 올릴 때 초기화 되는 대상을 저장하기 위한 메모리 공간
- Constant Pool
	- 전역 변수들 저장
	- 각 클래스/인터페이스 마다 별도의 constant pool 테이블이 존재하는데, 클래스 생성할때 참조해야할 정보들을 상수로 가지고 있는 영역
- Field information
	- 클래스의 필드 정보 보관
- Method Information
	- 클래스의 메소드들에 대한 정보 보관
- Type Information
	- Class인지 Interface인지 여부 저장, Type의 속성, Super Class의 이름
- Method Code
	- 메소드의 바이트 코드가 저장되는 공간
- Exception Table
	- 메서드 내에서 발생하는 예외에 대한 처리 정보 보관

#### 저장되는 곳 예시
```java
public class Student {
    private static int studentCount = 0; // Constant Pool
    private String name; 
    private int age;
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        studentCount++;
    }
    
    public void study() {
        String subject = "Math";
        int hours = 2;
        System.out.println(name + " studied " + subject + " for " + hours + " hours");
    }
    
    public static void main(String[] args) {
        Student student1 = new Student("Alice", 20);
        Student student2 = new Student("Bob", 21);
        student1.study();
    }
}
```