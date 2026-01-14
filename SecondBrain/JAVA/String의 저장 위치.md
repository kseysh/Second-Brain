## 문자열 "abc"의 저장 방식
```java
String s1 = "abc";
```
JVM이 "abc"를 String Constant pool(Heap 내부)에서 탐색
없으면, Pool에 저장
있으면, 같은 객체 참조 재사용 (이로 인해 비교 시 같다고 할 수 있음)
## new String("abc")
- `"abc"` 리터럴
    - String Pool에 1개 존재
- `new String("abc")`
    - **Heap에 새 String 객체 생성**
    - Pool과 다른 객체

