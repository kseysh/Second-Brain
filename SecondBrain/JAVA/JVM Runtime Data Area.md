## 스레드 영역
### 네이티브 메소드 영역
### PC 레지스터 영역
### 스택 영역
  
## 공유 영역
### 힙 영역
### 메서드 영역 (Metaspace Area (Java 8 ~))
JVM이 시작될 때 생성되는 공간으로 바이트 코드를 처음 메모리 공간에 올릴 때 초기화 되는 대상을 저장하기 위한 메모리 공간
- Constant Pool
	- 전역 변수들 저장
- Field information
	- 클래스의 필드 정보 보관
- Method Information
	- 클래스의 메소드들에 대한 정보 보관
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