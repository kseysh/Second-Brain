
```java
List<String> list = new ArrayList<>(); List<String> list2 = new ArrayList<>(); 

//요소 삽입 
list.add("one"); 

//특정 인덱스에 요소 삽입
list.add(0, "zero"); 

//리스트 병합 (추가되는 리스트가 뒤로 온다) 
list.addAll(list2); 

//특정 요소의 첫번째 인덱스 반환 
list.indexOf("zero"); 

// 0 //특정 요소의 마지막 인덱스 반환 
list.lastIndexOf("zero"); 

//특정 인덱스의 값 삭제 
list.remove(0); 

//첫번째 값 삭제 
list.remove("one"); 

//리스트 차집합 
list.removeAll(list2); // list에서 list2에 있는 모든 값을 삭제 

//리스트 교집합 
list.retainAll(list2); // list에서 list2에 있는 값을 제외한 모든 값을 삭제 

//리스트 비우기 
list.clear(); 

//리스트 비어있는지 체크 
list.isEmpty(); 

//리스트 길이 
list.size(); 

//리스트 특정 요소 포함여부 체크 
list.contains("one"); 

//리스트에 다른 리스트 요소가 전부 포함되어있는지 여부 체크 
list.containsALL(list2); // list에 list2의 모든 값이 포함되어 있으면 true 

//람다식 사용하여 요소들 제거 
list.removeIf(x -> x % 2 == 0) // list에서 짝수인 수를 모두 제거

```