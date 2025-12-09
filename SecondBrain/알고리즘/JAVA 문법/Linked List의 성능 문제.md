LinkedList는 데이터를 추가할 때마다 Node라는 Wrapper Class를 새로 생성한다. (추가적인 객체 생성이 이루어진다)
노드들이 힙 메모리에 산발적으로 위치하여 캐시 지역성이 떨어진다.

그에 비해 ArrayList, ArrayDeque는 내부적으로 Array를 사용한다.
또한, Wrapper Class인 Node를 사용하지 않아 메모리를 적게 사용한다.