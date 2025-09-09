String은 불변 객체이므로 String + String을 하게 되면 O(n + m)의 시간이 걸린다.
String 모두를 순회하고 이 값을 새로운 String에 할당하는 것이기 때문

하지만, StringBuilder같은 경우 `char[]`와 비슷한 매커니즘이기 때문에, 단순히 배열에 값 하나를 추가하는 것과 같아 O(1)의 시간 복잡도를 가진다.

하지만, 요즘은 JVM의 최적화 덕분에 String + String도 StringBuilder로 변환하여 전달한다고 한다.