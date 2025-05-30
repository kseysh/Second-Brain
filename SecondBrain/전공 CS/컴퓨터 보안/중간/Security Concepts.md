## 핵심 개념: 공격, 서비스, 메커니즘
![[Pasted image 20250306112437.png|400]]
그림 꼭 기억하기!
Message authentication: 둘 다 공개키 사용으로 부인 방지 기능 없음
Digital signature: 송신자는 개인키 수신자는 공개키 사용으로 송신자만 개인키를 알고 있어 송신자의 부인 방지 기능 있음
## 정보 보안의 정의 (NIST 기준)
정보와 정보 시스템을 무단 접근, 사용, 공개, 방해, 수정 또는 파괴로부터 보호하여 다음을 보장하는 것:
(A) 무결성(Integrity):
	•	부적절한 정보 수정이나 파괴를 방지하는 것
	•	정보의 부인 방지(non-repudiation)와 신뢰성(authenticity) 보장 포함
(B) 기밀성(Confidentiality):
	•	허가된 제한 내에서 접근과 공개를 보호하는 것
	•	개인 정보 및 독점 정보 보호 포함
(C) 가용성(Availability):
	•	정보에 대한 신속하고 신뢰할 수 있는 접근 및 사용을 보장하는 것
## 필수 정보 및 네트워크 보안 목표 5가지
- Confidentially
- Integrity
- Authenticity
- Availablillity
- Accountability(e.g., Nonrepudiation)
## 보안의 과제(Challenges of Security)
•	보안은 단순하지 않음
•	보안 기능을 공격할 수 있는 여러 가지 위협 요소를 고려해야 함
•	특정 보안 서비스를 제공하는 절차가 직관적이지 않을 때가 많음
•	다양한 보안 메커니즘을 어디에 적용할 것인지 결정해야 함
•	지속적인 모니터링이 필요함
•	보안은 특정 알고리즘이나 프로토콜 하나만으로 이루어지지 않음
•	본질적으로 공격자 vs. 방어자의 두뇌 싸움이지만, 방어자가 불리한 구조
	•	공격자: 단 하나의 취약점만 찾아내면 됨
	•	방어자: 모든 취약점을 찾아 제거해야 함
•	보안 투자 효과는 보안 실패가 발생하기 전까지는 체감하기 어려움
•	강력한 보안이 종종 효율성과 사용자 친화성을 저해한다고 인식됨
## 보안 설계 원칙(Security Design Principles)
1. **Fail-safe Defaults** (실패 안전 기본값)
	•	접근 제어는 거부를 기본값으로 설정해야 함
2. **Isolation** (격리)
	•	공개 접근 시스템은 중요 자원과 분리되어야 함 (정보 유출 및 변조 방지)
	•	개별 사용자의 프로세스 및 파일은 명확하게 격리되어야 함
	•	보안 메커니즘도 외부 접근으로부터 격리되어야 함
3. **Complete Mediation** (완전한 중재)
	•	모든 접근 요청은 접근 제어 메커니즘을 통해 검사되어야 함
4. **Open Design** (개방형 설계)
	•	보안 메커니즘의 설계는 비밀이 아니라 공개적으로 검토 가능해야 함
	•	암호화 키는 비밀이어야 하지만, 암호화 알고리즘은 공개되어야 함
		- 이유: 아무리 전문가여도 한계가 존재하므로 많은 전문가의 의견을 받아 안전하게 만들기 위해
5. **Least Privilege** (최소 권한 원칙)
## OSI 보안 아키텍처(Security Architecture in OSI)
1. 보안 공격(Security Attack)
	•	조직이 소유한 정보의 보안을 위협하는 모든 행위
2. 보안 메커니즘(Security Mechanism)
	•	보안 공격을 탐지, 방지 또는 복구하는 데 사용되는 프로세스 또는 장치
3. 보안 서비스(Security Service)
	•	데이터 처리 시스템과 정보 전송의 보안을 강화하는 서비스
	•	보안 공격을 방지하고, 하나 이상의 보안 메커니즘을 활용하여 보호
## 위협과 공격(Threats and Attacks)
1. 위협(Threat)
	•	조직 운영에 악영향을 미칠 수 있는 모든 상황 또는 사건
2. 공격(Attack)
	•	정보 시스템의 자원 또는 정보를 수집, 방해, 차단, 변조, 파괴하려는 모든 악의적 행위
## 보안 공격(Security Attacks)
•	수동 공격(Passive Attack):
	•	시스템의 정보를 훔쳐보거나 활용하려는 시도이지만, 시스템 자원에는 영향을 주지 않음
•	능동 공격(Active Attack):
	•	시스템 자원을 변경하거나 시스템 운영에 영향을 미치려는 시도
### 수동 공격(Passive Attacks)
•	전송 중인 데이터를 엿듣거나 감시하는 행위
•	공격자의 목표는 전송되는 정보를 획득하는 것
•	수동 공격의 두 가지 유형:
1.	메시지 내용 유출(Release of Message Contents)
2.	트래픽 분석(Traffic Analysis)
### 능동 공격(Active Attacks)
•	데이터 스트림을 수정하거나 가짜 데이터 스트림을 생성하는 공격
•	물리적, 소프트웨어적, 네트워크적 취약점이 다양하기 때문에 완벽한 방어가 어렵지만, 탐지 및 복구가 중요함
•	주요 공격 유형:
1.	신원 가장(**Masquerade**)
•	한 개체가 다른 개체인 척하는 공격
•	일반적으로 다른 형태의 능동 공격과 함께 수행됨
2.	재전송 공격(**Replay**)
•	데이터를 가로챈 후, 이를 다시 전송하여 Unauthorized effect를 유발
3.	데이터 수정(**Data Modification**)
•	정당한 메시지의 일부를 변경하거나,
•	메시지를 지연시키거나 순서를 바꿔서 무단 효과를 유발
4.	서비스 거부 공격(**Denial of Service, DoS**)
•	통신 시설의 정상적인 사용 또는 관리 기능을 방해하거나 차단
## Service 1: Authentication
•	통신이 신뢰할 수 있음을 보장하는 것과 관련됨
	•	단일 메시지의 경우, 수신자가 해당 메시지가 주장하는 출처에서 왔음을 보장함
	•	지속적인 상호작용의 경우, 두 개체가 신뢰할 수 있으며 제3자가 개입하여 두 개체 중 하나로 가장할 수 없음을 보장함
### X.800에서 정의된 두 가지 특정 인증 서비스
동료 개체 인증(Peer Entity Authentication) - 통신을 하는 사람이 정말 그 사람이 맞는지?
	•	연결 내에서 동료 개체의 신원을 확인하는 기능을 제공함
		•	동료 개체: 서로 다른 시스템에서 동일한 프로토콜을 구현하는 개체
	•	데이터 전송 단계의 시작 시 또는 도중에 사용되며, 개체가 가장하거나 이전 연결을 무단으로 재사용하는 것을 방지하는 데 도움을 줌
데이터 출처 인증(Data Origin Authentication) - 이 데이터가 그 사람에게서 온게 맞는지?
	•	데이터 단위의 출처를 확인하는 기능을 제공함
	•	데이터 단위의 중복 또는 수정에 대한 보호를 제공하지 않음
	•	전자 메일과 같이 지속적인 상호작용이 없는 애플리케이션을 지원함

## 서비스 2: 접근 제어(Access Control)
•	통신 링크를 통해 호스트 시스템 및 애플리케이션에 대한 접근을 제한하고 제어하는 기능
•	이를 위해, 접근을 시도하는 각 개체는 **Identified, or authenticated**여야 한다. 
이를 통해 개별적인 접근 권한을 부여할 수 있음
- Identification - 식별
- Authentication - 인증

### 사용자 인증

| 인증 요소                 | 예시                  | 특징                                             |
| --------------------- | ------------------- | ---------------------------------------------- |
| 지식(Knowledge)         | 사용자 ID, 비밀번호, PIN   | 공유, **관찰, 기록 가능** / 쉬운 비밀번호는 추측 가능 / 잊어버릴 수 있음 |
| 소지(Possession)        | 스마트 카드, 전자 배지, 전자 키 | 공유 가능 / 복제(클론) 가능 / 분실 또는 도난 가능                |
| 고유성(Inherence, 생체 정보) | 지문, 얼굴, 홍채, 음성      | 공유 불가능 / 오탐 및 미탐 가능 / 위조 어려움 / **유출 시 복구 어려움** |

•	다중 요소 인증(Multifactor Authentication): 두 가지 이상의 인증 방식을 사용하는 것
•	예: PIN + 하드웨어 토큰 / PIN + 생체 인증

## 서비스 3: 데이터 기밀성(Data Confidentiality)
•	수동 공격으로부터 전송 데이터를 보호함
•	가장 포괄적인 서비스는 특정 기간 동안 두 사용자 간 전송된 모든 사용자 데이터를 보호함
•	좁은 범위의 서비스는 단일 메시지 또는 특정 필드만 보호할 수도 있음
•	트래픽 분석으로부터 트래픽 흐름을 보호함
•	공격자가 출처 및 목적지, 빈도, 길이 또는 기타 특성을 관찰할 수 없도록 함

## 서비스 4: 데이터 무결성(Data Integrity)
- 데이터를 변조할 수 없어야 한다.
•	메시지 스트림, 단일 메시지 또는 특정 필드에 적용 가능
•	연결 지향 무결성(Connection-Oriented Integrity): 메시지 스트림을 다루며, 메시지가 중복, 삽입, 수정, 재정렬, 재전송 없이 전송된 그대로 수신됨을 보장함 ex) TCP
•	연결 없는 무결성(Connectionless Integrity): 개별 메시지를 다루며, 메시지 변경 방지에 중점을 둠 ex) UDP

## 서비스 5: 부인 방지(Nonrepudiation)
•	발신자 또는 수신자가 전송된 메시지를 부인할 수 없도록 함
•	발신자는 수신자가 해당 메시지를 실제로 받았음을 증명할 수 있음
•	수신자는 발신자가 해당 메시지를 실제로 보냈음을 증명할 수 있음

## 서비스 6: 가용성 서비스(Availability Service)
•	시스템의 가용성을 보호함
•	서비스 거부(DoS) 공격으로 인한 보안 문제를 해결함
•	접근 제어 서비스 및 기타 보안 서비스의 적절한 관리와 제어에 의존함
## 네트워크 보안의 핵심 요소
![[Pasted image 20250311104212.png|400]]
## Communications Security
네트워크를 통한 통신의 보호를 다루며, 수동적 공격과 능동적 공격 모두에 대한 방어 조치를 포함
통신 보안은 주로 **네트워크 프로토콜**을 사용하여 구현

IPsec : IP secure
TLS : Transport Layer Security (= Secure Socket Layer, SSL)
SSH: Secure shell
S/MIME: 이메일에 추가 기능을 준 것: MIME, Secure MIME
IEEE 802.11i: wifi 표준
## Device Security
- Firewall (방화벽)
	- 하드웨어 및/또는 소프트웨어 기능으로, 특정 보안 정책에 따라 네트워크와 네트워크에 연결된 장치 간의 접근을 제한
- Intrusion detection (침입 감지)
	- Host-based(**HIDS**) vs. NW-based(**NIDS**)
- Intrusion prevention (침입 방어)
## Standards
- National Institute of Standards and Tecnology (NIST)
	- 미국 국립표준기술연구소 아래 두 개는 국가적 범위이지만, 전 세계에 영향을 미침
	- FIPS
	- SP
- Internet Society (ISOC)
	- 조직 및 개인 회원으로 구성된 전문가 협회
	- IETF - Internet Engineering Task Force
	- RFCs - 인터넷 표준 및 관련 명세
- ITU-T
	- UN 산하 국제 기구
	- 전기통신 기술 표준 개발담당
- ISO (International Organization for Standardization)
	- 국제 표준화 기구
- IEEE (Institute of Electrical and Electronics Engineers)
	- 미국 전기전자 학회
- TTA (Telecommunications Technology Association)
	- 한국정보통신기술협회
