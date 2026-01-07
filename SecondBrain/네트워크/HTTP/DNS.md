## DNS의 두 가지 의미
- Domain name을 IP주소로 변활할 때 쓰는 레코드들을 보관하는 분산형 데이터베이스 서버
- 클라이언트가 서버랑 메시지를 주고받을 수 있게 해주는 프로토콜
## IP를 얻는 과정
1. 브라우저의 URL 파싱
2. 브라우저 캐시 탐색
3. Stub Resolver(브라우저의 캐시)가 OS의 DNS 캐시 및 hosts 파일 확인
4. Stub Resolver의 Recursive DNS 서버로 DNS 쿼리 패킷을 보냄 (Full Resolver로 DNS 프로토콜에 따라 요청을 보냄)
5. Full Resolver (or Recursive Resolver)는 Root 서버 -> TLD(Top-Level Domain) 서버 -> 책임 DNS 서버 순으로 반복적 질의 과정을 수행함
	1. 대부분 통신사 ISP DNS, 공용 DNS, 사내 DNS가 이 역할을 수행함
