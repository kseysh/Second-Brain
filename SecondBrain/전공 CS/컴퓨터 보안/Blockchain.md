## 특징
- 오픈소스 소프트웨어
- 중앙의 관리자나 중재 서버 없이 peer-to-peer (P2P) 방식으로 거래 하는, 탈중앙화된 가상 자산
- 실제로 디지털화된 “coin”이 존재하지는 않으며, 참가자(peer) 간의 거래(송금 내역)를 기록하는 원장(ledger)만 존재
- **분산 원장**을 사용하여 다수 참여자가 동일한 공개 거래 기록을 관리, *무결성 제공되나 기밀성 제공 안됨*
	- 중앙 관리 데이터베이스는 여러 보안 수단을 구비한 은행 서버가 데이터를 관리하므로 기밀성과 무결성을 제공함
## Bitcoin Transaction
Transaction: 거래의 흐름 (가상 자신의 전송)
bitcoin address: 송신자와 수신자는 실명 대신, 일종의 가명(bitcoin address) 사용 (= public key for ECDSA)
### chain of transactions
- Ledger에는 잔액이 표시되어 있지 않음 
- 모든 거래 내역이 공개되어 있기 때문에 이를 통해 잔액을 계산할 수 있음 
	- 이전 거래에서 누군가에게 받은 자산 중 아직 소비하지 않은 자산 (Unspent Transaction Output: UTXO)들을 종합
![[Pasted image 20250604220407.png|300]]
CA로부터 인증서를 받아 검증하는 것이 아닌, 이미 모든 사람들이 다 동의했어서 누구나 복사해서 가지고 있는 이미 검증된 Transaction안의 Public key를 이용해서 내용을 검증한다.
![[Pasted image 20250531141916.png|300]]
부분적인 지불 관계도 표현 가능
잔액을 일종의 거스름돈에 해당하는 transaction으로 표현
## Bitcoin address
일종의 가명
- 비트코인 주소는 ID인 동시에 전자서명 확인용 public key임 
- 비트코인의 수신자를 비트코인 주소로 명시 
- 비트코인의 송신자(지급자)는 연결된 이전 트랜잭션에 포함되었던 비트코인 주소(즉, 공개키)에 대응되는 개인 키를 이용하여 전자서명을 수행함으로써 지급 트랜잭션 완성 
- 모든 참여자들은 이전 트랜잭션에 포함된 공개키를 이용 하여 이 서명을 확인함으로써 트랜잭션 유효성 검증
## Bitcoin wallet
- 이전 트랜잭션에 포함된 공개키에 대응되는 개인키를 이용하여 서명해야 하므로, 개인키 분실시 소유권 증명 불가 
- 개인키 저장을 위해 비트코인 지갑(wallet) 사용 
- 지갑은 또한 블록에 기록된 트랜잭션들을 추적하여 본인의 잔고를 확인하는 기능도 제공 가능
![[Pasted image 20250531142614.png|400]]
## Bitcoin block
트랜잭션 여러 개를 모아 순서에 따라 기록한 것
![[Pasted image 20250531143709.png|400]]
![[Pasted image 20250531143813.png|400]]
이전 블록의 hash: 이전 블록이 다 완성되어서 그 후에 생성된 블록이라는 뜻
# Blcok chain
- Block들 간의 의존 관계 성립 
- 이전 블록 해시 값을 다음 블록에 포함 
- 특정 블록을 조작할 경우, 그 해시값에 의해 영향을 받는 다음 블록도 조작하여야 함 (그래서 블록의 조작이 어려움)
- 해시함수의 특성(one-way property, collision resistance): 해당 해시값이 변하지 않도록 하면서 조작 대상 블록의 다른 값 (예: nonce)을 같이 변경시키거나, 다른 트랜잭션들을 포함하면서 같은 해시값을 가지도록 하는 것은 거의 불가능함 
- 변경의 영향은 체인처럼 연결된 다음 블록들로 계속 전파되므로 맨 뒤 블록까지 모두 수정해야 함 
- 트랜잭션이 포함된 투명한 상자(블록)들의 탑(블록체인)의 복제 본이 수많은 참여자(노드)들 에게 저장되어 있으므로 조작이 더 어려움
### Bitcoin network 
- Transaction을 수행하는 peer-topeer (P2P) 네트워크 
- 네트워크로 연결된 각 개체를 노드 (node)라 부름 
### 비트코인 노드(node) 
- Full client: 블록체인 전체를 보유 
- Lightweight client 
	- full client의 도움을 받음 
- Bitcoin Core 
	- node + wallet (Open source SW)
![[Pasted image 20250531144138.png|500]]
## Transaction and block generation
### Transaction 생성
■ 참여자들 중 비트코인 송금(전송)을 원하는 노드는 해당 내용을 포함한 트랜잭션을 생성
■ 이 트랜잭션을 주변의 노드들에게 동시 발송(broadcast)하여 추가 요청
![[Pasted image 20250601004653.png|400]]
■ 각 노드들은 주변의 가까운 노드들로부터 트랜잭션 추가 요청들을 받아, 트랜잭션 풀(transaction pool) 생성
■ 트랜잭션 풀은 검증 대기중인 트랜잭션들의 모음 (따라서 아직 거래가 완료된 것이 아님)
■ 트랜잭션 전파가 동시에 완료되지 않으므로, 각 노드들의 트랜잭션 풀 내의 트랜잭션 구성은 노드마다 다름
![[Pasted image 20250601004720.png|400]]
### Block 생성
각 노드는 트랜잭션 풀 내에 대기중인 트랜잭션들 중 일부를 선택하고, 이들을 포함한 새로운 블록 생성
![[Pasted image 20250601004755.png|400]]
경쟁하는 여러 블록체인 버전들 중 하나의 체인을 어떻게 결정?
먼저 만들어지는 블록이 승자가 됨
![[Pasted image 20250601004820.png|400]]
## Mining
작업 증명 (proof of work: PoW)
- SHA-256 수행
- 트랜잭션들과 이전 블록 hash 값은 정해져 있음
- 이들과 nonce를 입력으로 했을 때의 해시 결과가 미리 정해진 특정 목표 범위 (=current target) 이내가 되게 하는 nonce 결정
![[Pasted image 20250601004904.png|300]]
### 작업 증명 (proof of work: PoW)
- 해시 함수는 단방향 함수로, 출력(의 조건)으로부터 입력(의 조건)을 유추하기 어려우며, 무작위 함수처럼 동작함
- 따라서 조건을 만족하는 nonce를 찾는 유일한 방법은, 모든 가능한 nonce 값들을 무작위로 대입해 보는 것
- 이를 위해 해시 함수 계산을 위한 방대한 작업(work)이 필요하며, CPU, GPU, 전용 하드웨어 등 많은 자원이 필요함
채굴에 대한 보상 1: 코인베이스(coinbase) 트랜잭션
- 새로운 블록 생성 성공시 새로운 coin (coinbase = 새로운 통화) 생성
- 비트코인 초기(genesis block부터 21만번째 블록)에는 보상값이 50BTC(50 비트코인)였으나, 매 21만 개 블록(약 4년)마다 보상이 반감되도록 설계되었음
- 2025.5 현재 보상은 50/16 = 3.125BTC
채굴에 대한 보상 2: 거래 수수료 (transaction fee)
- 각 트랜잭션에 대해 transaction out의 합 < transaction input의 합 이며, 둘의 차이가 거래 수수료
- 채굴자가 블록 구성을 위한 트랜잭션 선택시, 거래 수수료가 높은 트랜잭션부터 선택 가능함
### Block 생성 취소
경쟁자들 중 하나의 노드만 채굴에 성공
나머지 노드들이 생성한 블록은 취소되며, 그 안에 포함되었던 트랜잭션들은 다시 풀로 돌아가 검증을 기다림
![[Pasted image 20250601005115.png|400]]
## Bitcoin 반감기와 통화량
- PoW 난이도 (PoW difficulty) 
	- 통화량의 증가 속도를 조절하기 위해, 새로운 블록은 10분마다 생성되도록 설정되어 있음
	- 만약 컴퓨터 자원의 투입이 늘어 채굴 속도가 빨라질 경우, 해시 함수의 정해진 목표값 범위를 좁혀서, 즉 난이도를 높여서 다시 블록 생성 속도가 10분 당 1블록이 되도록 자동적으로 조절됨
	- 단, 난이도의 조정 배율은 0.75 (난이도 하향) ~ 4.0 (난이도 상향)로 제한
- 반감기와 통화량
- 매 21만 개 블록마다 보상이 반감됨 (1블록/10분 = 144블록/일 = 52,560 블록/년 → 대략 21만 블록/4년)
-  반감이 33회 반복되면 채굴 보상은 ![[Pasted image 20250601005219.png|100]]이 되어, 비트코인 최소 통화단위인 1 satoshi(=0.00000001 BTC)보다 작아지게 됨
- 따라서, 2008년(genesis block) 이후 4×33=132년이 경과한 2140년이 되면 더 이상 통화량 증가 없게 됨
- 총 통화량은 ![[Pasted image 20250601005311.png|200]]에서 정체됨
## Bitcoin: Issues
- Scalability issue
	- Maximum block size: 1MiB(=2^20 B), transaction 의 크기는 평균 수백 B 또는 수 KB
- 따라서 block 당 transaction 은 수백~수천개
- Block이 10분에 하나 정도 생성되므로, transaction throughput은 초당 10개 이하
	- ➔ off-chain transactions (시작과 끝만 기록하는 느낌)
- Mining pool 집중 문제 (먼저 mining을 했음에도 불구하고 과반수가 이게 먼저야 라고 양으로 누르는 것)
- Full node vs. thin client
	- ➔ Merkle tree (다음페이지)
## Blockchain : Reclaiming Space using Merkle Tree
![[Pasted image 20250601005409.png|300]]
TX3을 보낼 때, Hash2와 Hash01을 같이 보내면 Root Hash를 검증할 수 있어 Tx3를 신뢰할 수 있다.
N개를 보내는 것이 아닌 logN개만 보내면 된다.
## Other blockchains
- Review: 비트코인의 동작 원리
	- 중앙의 관리자나 중재 서버 없이 peer-to-peer (P2P) 방식으로 거래하는, 탈중앙화된 가상자산(암호화폐)
	- 참가자(peer) 간의 거래(송금 내역)를 기록한 블록체인의 공개된 사본을 복제하여 다수의 참여자가 저장
	-  다수 참여자가 동일한 거래 기록을 관리함으로써 무결성 확보(임의 조작 방지)
- 블록체인 개념의 확장
	- 블록체인에 기록되는 내용이 화폐의 전송에 국한될 필요 없음
	- 컴퓨터 상에서 수행되는 프로그램을 “컴퓨터 메모리의 상태를 변화시키는 작업의 연속”으로 보면, 블록체인 네트워크를 하나의 방대한 가상화된 컴퓨터로 보고 공동의 프로그램을 수행시키는 것이 가능함
	- 즉 노드들은 블록체인의 상태(가상화된 컴퓨터의 메모리 상태)를 함께 변경시키는 작업을 계속함
	- 일종의 거대한 클라우드
### Ethereum
- 블록체인 네트워크를 이용하여 하나의 거대한 가상 컴퓨터를 운영하는 구조
- 가상자산인 동시에 거대한 가상 컴퓨터
	- EVM (Ethereum Virtual Machine)
	- Original implementation: in C++/Python/Golang
	- Geth: Go Ethereum
- 스마트 컨트랙트(smart contract): 이더리움 블록체인 상에서 실행되는 프로그램
- Solidity language
- 비트코인과는 다르게, Turing-complete 한 기능 제공(예: 반복문을 이용한 특정 작업 반복 가능)
- 비트코인의 트랜잭션 수수료와 같이, 프로그램의 실행에 대한 비용(gas) 지불해야 함
- Consensus (합의) mechanism: PoW에서 2022년 Proof-of-stake (PoS)로 전환 (less energy consumption, higher security) – “The Merge”

### 하이퍼레저(Hyperledger)
- 공개형 블록체인은 참여 노드가 많아질 수록 구조가 복잡하고 특히 PoW 방식의 경우 자원 낭비가 심하며, 거래 내역 및 데이터가 모두 공개되는 기밀성 문제가 있음
- 허가받은 소수만 블록체인에 참여하는 허가형 블록체인(permissioned blockchain) 등장. 단, 참여자들 간에 대립되는 이해관계가 있고(신뢰할 수 없는 복수의 기관이 참여) 중앙화가 적절하지 않은 경우 적합함(완전한 탈중앙화는 아님)
- Hyperledger Fabric
	- Hyperledger 오픈 소스 프로젝트(Sawtooth, Besu, etc.)에 포함된 대표적인 플랫폼
	- 대표적인 허가형 블록체인의 하나로, Linux 재단에서 시작하고 IBM 등의 기업에서 주도
### NFT
- NFT (Non Fungible Token: 대체 불가 토큰)
	- Blockchain에 기록된 metadata + ownership
	- Data의 직접 기록 대신 외부에 저장된 디지털 매체에 대한 reference를 저장하는 것이 일반적
	- Uniquely identifiable (“fungible”한 보통의 cryptocurrency와 다름)
- NFT 구성
	- On-chain smart contract: blockchain에 token을 minting, 전송, 소각하는 방법을 알려주는 명령의 집합
		- Ethereum 상에 구현하는 경우가 전형적
		- 최초의 NFT project라 볼 수 있는 Etheria는 2015년 Ethereum blockchain launch 직후 launch.
		- ERC (Ethereum Request for Comments) 721 standard (2018) “Non-Fungible Token Standard” 
		- ERC 1155 (2018): multi-token standard (handles multiple token types (fungible and non-fungible) in a single transaction)
- Off-chain: metadata + contents