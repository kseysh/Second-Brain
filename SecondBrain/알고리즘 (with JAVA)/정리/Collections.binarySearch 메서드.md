java.util.Collections.binarySearch() 메소드는 정렬된 리스트에서 탐색 대상의 index를 반환하는 java.util.Collections 클래스 메소드이다.

이진 탐색으로 값을 찾기 때문에 정렬이 되어 있지 않으면 이진 탐색을 할 수가 없다. 따라서 반드시 오름차순으로 정렬을 해준 다음, 해당 메소드를 호출해야 한다.

탐색 대상을 찾았을 땐, 그 대상의 위치를, 만약 찾지 못했을 땐 (-(insertion point) - 1) 에 해당하는 값을 리턴하게 된다.

insertion point는 해당 리스트에 탐색 대상 key가 만약 존재했다면 위치했을 index를 의미한다.

지정한 comparator를 사용해 리스트의 요소를 비교할 수 없을 때, key를 비교할 수 없을 때 ClassCastException이 발생할 수 있다.

```java
import java.util.List; 
import java.util.ArrayList; 
import java.util.Collections; 

public class TEST { 
	public static void main(String[] args) 
	{ 
		List al = new ArrayList(); 
		al.add(1); 
		al.add(2); 
		al.add(3); 
		al.add(10); 
		al.add(20); 
		
		// 10 은 현재 3번 인덱스에 존재한다. 
		int index = Collections.binarySearch(al, 10);
		System.out.println(index); 
		
		// 13 은 현재 없다. 13 은 4번 인덱스에 위치했을 것이다. 
		// 그래서 이 함수는 (-4-1) = -5 를 리턴하게 된다. 
		index = Collections.binarySearch(al, 13); 
		System.out.println(index);
	} 
} 
실행 결과 : 
3 
-5

```