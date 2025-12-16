asyncio 라이브러리와 함께 사용하기 위해 만들어진 PostgreSQL용 고성능 비동기 DB Driver
## 기능
- Connection Pool 제공
- Prepared Statement 제공
- Type Conversion 제공 (Array, Json을 Python 객체로 변환)
## 장점
- PostgreSQL의 표준 클라이언트 라이브러리인 `libpq`(C로 구현되어 있음)를 사용하지 않고, PostgreSQL의 바이너리 프로토콜을 Python으로 직접 구현했기 때문에 오버헤드가 적어 빠르다.
- async/await 문법을 기본적으로 지원하여, Blocking 없이 DB 작업을 처리할 수 있음
## 단점
- async 함수 내부에서만 사용해야 한다.

