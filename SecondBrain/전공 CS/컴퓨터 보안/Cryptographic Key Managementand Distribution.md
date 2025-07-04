## 목차
•	중간자 공격 (Man-in-the-Middle Attack)
•	공개키 배포 방식
	•	(공개키의 공개 발표)
	•	공개 디렉터리 이용 (공개키 게시)
	•	공개키 권한기관(PK authority)에 의한 공개키 배포
	•	공개키 인증서 교환
•	X.509 인증서
•	공개키 기반구조(PKI)
## Man in the Middle Attack
![[Pasted image 20250529152503.png|400]]

## 그림 15.7: 공개키 게시
![[Pasted image 20250529152516.png|300]]
•	신뢰할 수 있는 기관 또는 조직이 운영하는 공개 디렉터리 사용
•	각 참여자는 자신의 공개키를 디렉터리 권한기관에 등록
•	등록은 직접 대면하거나 보안 인증된 통신 수단을 통해 이뤄져야 함
## 그림 15.8: 공개키 배포
•	가정: 모든 참여자는 권한기관의 공개키를 신뢰할 수 있음
•	단점: 공개키 권한기관(PK authority)이 병목지점이 될 수 있음
![[Pasted image 20250529152538.png|500]]
## 그림 15.9: 공개키 인증서 교환
•	참여자들은 PK 권한기관과의 직접 통신 없이 서로의 키를 교환
•	마치 권한기관에서 직접 받은 것만큼 신뢰성 있게 키를 획득 가능
![[Pasted image 20250529152558.png|300]]
## X.509 인증서
•	X.500 시리즈 권고안의 일부로서, 디렉터리 서비스를 정의함
	•	디렉터리는 사용자 정보를 관리하는 서버 또는 분산 서버 집합
•	X.509는 X.500 디렉터리를 기반으로 한 인증 서비스 프레임워크
	•	최초 발행: 1988년
	•	최신 개정 사항:
		▪ 9판: X.509 (2019.10)
		▪ 9.1판: 기술 정오표 1 (2021.10)
		▪ 9.2판: 기술 정오표 2 (2023.10)
		▪ 9.3판: 부속서 1: 기타 개선 사항 (2024.10)
	•	공개키 암호와 디지털 서명 기반
	•	특정 알고리즘 사용은 강제하지 않음
•	각 인증서는 사용자의 공개키를 포함하고, 신뢰된 인증기관(CA)의 개인키로 서명
•	사용 예시: IPSec, TLS, 공동인증서 등
## X.509 Public-Key Certificate Use
![[Pasted image 20250529152854.png|400]]
## X.509 Formats
![[Pasted image 20250529152920.png|400]]
## MitM Attack 대응책
![[Pasted image 20250529152949.png|400]]
1. CA-> Alice 인증서 발급
	1. C = S<sub>PR<sub>CA</sub></sub>(A, PU<sub>A</sub>)
	2. CA의 공개키 PU<sub>CA</sub>로 검증 가능
2. Alice -> Bob 서명 및 키 전달 (Y<sub>A</sub>, C, Z 전달)
	1. Alice는 Y<sub>A</sub>=a<sup>X<sub>A</sub></sup> mod q를 생성
	2. Alice는 이 키에 대해 자신의 개인키로 서명함 Z = S<sub>PR<sub>A</sub></sub>(A,Y<sub>A</sub>)
3. Bob: 인증서 및 서명 검증
	1. PU<sub>CA</sub>로 인증서 C를 검증해 PU<sub>A</sub>획득
	2. PU<sub>A</sub>로 서명 Z 검증

---
1. CA가 Alice의 공개키를 담은 인증서 발급
2. Alice는 자신의 개인키로 서명한 Y<sub>A</sub>를 담은 Z와 Y<sub>A</sub> C를 보냄
3. Bob은 PU<sub>CA</sub>로 C 검증해 PU<sub>A</sub>로 Z 검증 => 보낸 Y<sub>A</sub>와 Z안의 Y<sub>A</sub>가 같으면 신뢰
## 그림 15.12: X.509 계층 구조 – 가상의 예시
![[Pasted image 20250529153011.png|500]]
•	표기법: X<\<C>>는 X가 C에 의해 서명된 인증서를 의미
(즉, 𝑆<sub>PR<sub>X</sub></sub>(𝐶, 𝑃𝑈<sub>𝐶</sub>))
## 인증서 폐기 (Certificate Revocation)
•	각 인증서에는 유효 기간이 포함됨
	→ 일반적으로 만료 직전에 새 인증서 발급
•	그러나 다음과 같은 이유로 만료 전 폐기가 필요할 수 있음:
	•	사용자의 개인키가 유출된 것으로 의심
	•	사용자가 더 이상 해당 CA에 의해 인증되지 않음
	•	해당 CA의 인증서가 손상된 것으로 의심
•	각 CA는 폐기되었으나 아직 만료되지 않은 인증서 목록(CRL)을 유지해야 함
	•	이 목록은 디렉터리에 게시되어야 함
## Public Key Infrastructure
![[Pasted image 20250529153209.png|400]]