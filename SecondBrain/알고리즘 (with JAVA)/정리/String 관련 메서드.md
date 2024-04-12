
```java
String str = "apple"; 

//길이 반환 
str.length(); 

//빈 문자열 체크 
str.isEmpty(); 

//문자 찾기 
str.charAt(0); 

// 'a' -> 문자 반환 
str.indexOf("a"); 

// 0 -> 인덱스 반환 
str.lastIndexOf("p") 

// 2 -> 마지막으로 문자가 속한 인덱스 반환 
//문자 자르기 
str.substring(1, 3); 
// "pp" -> 인덱스 1 이상 3 미만 위치의 문자열 반환 
str.substring(3); 
// "app" -> 인덱스 3 미만 위치의 문자열 반환 
//문자 치환(바꾸기) 

//replace([기존문자],[바꿀문자]) 
str.replace('p', 'e');  // "aeele" -> 모든 [기존 문자]를 [바꿀 문자]로 치환 

//replaceAll([정규식],[바꿀문자]) 
str.replaceAll(".", "/");  // "/////" -> 정규식에 맞춰 문자 치환 (정규식 "." 은 모든 문자를 의미) 

//replaceFirst([기존문자],[바꿀문자]) 
str.replaceFirst('p', 'e'); // "aeple" -> 여러 문자 중 첫번째만 치환 

//문자 동일 여부 판단 
//자바 string의 경우, 클래스로써 Call by Reference형태로 생성 시 주소값이 부여된다. 
//그렇기에 String타입을 선언했을때는 같은 값을 부여하더라도 서로간의 주소값이 다르다. 
//따라서 값 비교로는 equals를 사용한다. 
str.equals("apple"); 

//문자 비교 
/** 
* str과 applf가 같으면 0 * str이 applf보다 사전순으로 앞이면 -1 
* str이 applf보다 사전순으로 뒤면 1 
* str과 applf가 마지막 문자만 다르면, 마지막 문자의 사전순 차이 반환 
*/
str.compareTo("applp"); // -1 -> 위 내용 참고 

//문자 포함 여부 판단 
str.contains("app"); 

//문자열 분리 
str.split(" "); 

//공백으로 구분된 문자열 str을 분리하여 String[] 배열로 반환 
str.split(); 

//띄어쓰기 없는 문자열 str을 한 문자씩 분리하여 String[] 배열로 반환 //문자 앞뒤 공백 제거 
str.trim();  // str의 앞뒤 공백을 제거한다. 문자열 사이의 공백은 제거하지 않는다. 

//문자 <-> 숫자 변환 
Integer.parseInt("100") //문자열 "100"을 숫자 100으로 변환 
Integer.toString(100) //숫자 100을 문자열 "100"으로 변환

```