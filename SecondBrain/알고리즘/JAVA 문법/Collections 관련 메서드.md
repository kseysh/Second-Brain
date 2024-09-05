```java
int[] temp = {1, 2, 3, 10, 20}; 
List<Integer> list = new ArrayList<>(Arrays.asList(arr));

//정수형 List 원소 중 최대, 최소값
Collections.max(list); 
Collections.min(list); 

//List 정렬 
Collections.sort(list); // 오름차순 (ASC) 
Collections.sort(list, Collections.reverseOrder()) // 내림차순 (DESC) 

//List 뒤집기 
Collections.reverse(list); 

//List 내 원소의 갯수 반환 
Collections.frequency(list, 3); 

//List 내 원소를 이진탐색을 이용해 찾기 
Collections.binarySerch(list, 10); // 3

```