- HashMap : <key, value>쌍, value의 중복 허용 O, 순서 X
- LinkedHashMap : <key, value>쌍, value의 중복 허용 O, key 순서 O (삽입순)
- TreeMap : <key, value>쌍, key 순서가 오름차순(알파벳순) 으로 정렬됨

```java
HashMap<Integer, String> hashMap = new HashMap<>(); 

//요소 추가 
hashMap.put(1, "딸기"); 
hashMap.put(2, "바나나"); hashMap.put(1, "사과"); // (1. "딸기")의 value가 "사과"로 대체됨 //요소 삭제 hashMap.remove(1); // key 가 1인 요소 삭제 //전체 삭제 hashMap.clear(); //key 포함 여부 확인 hashMap.containsKey(1); //value 포함 여부 확인 hashMap.containsValue("사과"); //key - value 출력 for (Integer key : hashMap.keySet()){ System.out.println(key + " " + hashMap.get(key)) } for (Entry<Integer, String> temp : hashMap.entrySet()){ System.out.println(temp.getKey() + " " + temp.getValue()); }
```