만약 상속하는 조상이 없다면 자동적으로 Object 클래스를 상속받게 된다.
모든 class는 Object 클래스에 정의된 11개의 메서드를 상속받는다.

1. `protected Object clone( )` : 객체 자신의 복사본을 반환한다.|

2. `public boolean equals(Object obj)` : 객체 자신과 객체 obj가 같은 객체인지 알려준다 (같으면 true)

3. `protected void finalize( )` : 객체가 소멸될 때 가비지 컬렉터에 의해 자동적으로 호출되어 객체가 소멸되기 직전에 데이터들을 반납한다.  <br>그래서 이 메서드를 오버라이딩 해서 데이터를 청소해 주어야 겠지만 하지만 요즘은 다른방법으로 청소하여 안쓰인다  (더 이상 권장되지 않는 메서드)

4. `public Class getClass( )`객체 자신의 클래스 정보(설계도)를 담고있는 Class 인스턴스를 반환  <br>다른 곳에서 해당 객체의 정보를 가지고 객체를 재생성 해야 할때 이용됨

5. `public int hashCode( )` : 객체 자신의 해시코드를 반환  <br>※ 해시코드는 메모리 주소를 int형으로 변환한 값

6. `public String toString( )` : 객체 자신의 정보를 문자열로 반환

### 아래부터 쓰레드용 메서드

7. `public void notify( )` : (쓰레드용 메서드)  <br>객체 자신을 사용하려고 기다리는 쓰레드를 하나만 깨운다.

8. `public void notifyAll( )` : (쓰레드용 메서드)  <br>객체 자신을 사용하려고 기다리는 모든 쓰레드를 깨운다.

9. `public void wait( )`  <br>
10. `public void wait(long timout)`  <br>
11. `public void wait(long timeout, int nanos)` : (쓰레드용 메서드)  <br>른 쓰레드가  notify() 나  notifyAll() 을 호출할 때까지 현재 쓰레드를 무한히 또는 지정된 시간(timeout, nanos)동안 기다리게 한다.(timeout은 1/1000초, nanos는 1/(10^9)초)|