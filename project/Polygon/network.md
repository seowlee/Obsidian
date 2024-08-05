# Polygon

Layer-2 (L2) network
Consensus(합의방식) : Proof-of-Stake (PoS)
이론적으로 65000 TPS (Real-Time TPS 55.65 tx/s)
Block Time(블록생성시간) : 2.25초
Time-to-finality : 4분16초
372 confirmations 20분 (124 의 3배)
450 confirmations 9분?
127
100 moralis

이더리움의 사이드체인(L2의 한 종류)으로 시작. 인도에서 시작됨
[폴리곤(Polygon)](https://www.btcc.com/ko-KR/academy/crypto-basics/what-is-polygon)은 이더리움의 확장성 문제를 해결하고자 나온 프로젝트로, 이더리움 플라즈마 프레임워크를 개선한 솔루션을 2017년에 가지고 나왔습니다

폴리곤은 원래 M&A를 통해 확장을 하기로 유명. 
5400억 투자 받은 돈으로 ZK 롤업쪽 회사들을 어마무시하게 사들이기 시작했다고 함.

출처: [https://jonnabutija.tistory.com/entry/문과생도-이해하는-폴리곤MATIC-분석-1편-웹3의-AWS](https://jonnabutija.tistory.com/entry/%EB%AC%B8%EA%B3%BC%EC%83%9D%EB%8F%84-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-%ED%8F%B4%EB%A6%AC%EA%B3%A4MATIC-%EB%B6%84%EC%84%9D-1%ED%8E%B8-%EC%9B%B93%EC%9D%98-AWS) 

# MATIC
폴리곤은 2019년 4월 토큰(MATIC) 론칭
최대 공급량은 100억 MATIC. 100% 공급됨. 총 공급량의 98.99% 가 시장에 유통되어있음 (23년 12월 기준)
1 MATIC =  10^18 Wei (1 Ether의 하위 단위와 동일한 방식) [unitconverter](https://polygonscan.com/unitconverter)
기본 토큰 티커 MATIC -> POL 변경 될것 ( 대체 되는 중. 1:1 swap)

# Network 종류

## Polygon PoS  vs Polygon zkEVM

| **Polygon PoS vs Polygon zkEVM** | **Polygon PoS**                                            | **Polygon zkEVM**                                                               |
| -------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **Architecture**                 | Polygon PoS는 PoS 합의 메커니즘과 Plasma 프레임워크를 활용하여 사이드체인을 생성합니다. | Polygon zkEVM은 zk-rollup 아키텍처를 특징으로 하며, 영지식 증명을 활용하여 Ethereum 위에 L2 솔루션을 제공합니다. |
| **Consensus Mechanism**          | PoS                                                        | Consensus Contract                                                              |
| **Smart Contract Compatibility** | EVM Compatibility                                          | EVM Equivalence                                                                 |
| **Security**                     | Polygon PoS는 사이드체인을 보호하는 독립적인 검증자 집합을 특징으로 합니다.            | Polygon zkEVM은 유효성 증명이 온체인에 저장되므로 Ethereum에 의해 보호됩니다.                           |
| **Data Availability**            | 데이터는 사이드체인에서 직접 사용할 수 있습니다.                                | 데이터 가용성에는 Validium과 Volition의 두 가지 옵션이 있습니다.                                    |

### Polygon PoS (Mainnet)

이더리움 블록체인을 위한 레이어 2(L2) 확장 솔루션.
#### Architecture
이 네트워크는 PoS 합의 메커니즘과 "플라즈마(Plasma)" 프레임워크를 활용하여 이더리움 메인넷과 병렬로 실행되는 PoS 사이드체인으로 기능한다.
이 별도의 네트워크를 통해 Polygon PoS는 이더리움 네트워크의 확장성 문제를 해결하고, 거래 수수료를 낮추며 처리량을 크게 증가시킬 수 있다
#### Consensus Mechanism
Polygon PoS는 지분 증명(PoS) 합의 메커니즘을 사용하며, 사이드체인에서 거래를 확인하고 검증하는 역할을 하는 검증자 집합에 의존합니다. 네트워크에는 약 100명의 검증자가 있으며, 이들은 MATIC 토큰을 스테이킹해야 참여 자격을 얻을 수 있습니다. 또한, 블록 생성자는 스테이킹한 양에 따라 선택됩니다.
#### Smart Contract Compatibility
Polygon PoS는 EVM 호환성을 제공하여 사이드체인에 이더리움 스마트 계약을 배포하고 실행할 수 있습니다. 그러나 EVM 호환성은 사이드체인과 이더리움 메인넷 사이의 실행 환경에 약간의 차이가 있을 수 있음을 의미합니다. 따라서 저수준 코드나 복잡한 dApp을 다룰 때는 Ethereum에서 Polygon PoS로 이동할 때 smart contract를 Polygon PoS에 맞게 수정이 필요할 수 있습니다.
#### Security
Polygon PoS는 사이드체인을 보호하기 위해 그 자신의 검증자들에게 매우 의존하며, 이는 이더리움 메인넷과 독립적으로 운영됩니다.
#### Data Availability
Polygon PoS에서 상태 및 모든 트랜잭션 데이터는 사이드체인에서 직접적으로 사용할 수 있습니다. 이는 빠른 데이터 검색과 상호 참조를 가능하게 하지만, 추가 저장 공간 비용이 발생할 수 있습니다.
#### Use Cases
Polygon PoS는 높은 처리량과 매우 낮은 수수료를 제공합니다. 이로 인해 Polygon PoS는 높은 트랜잭션 처리량이 필요한 dapp들이 이상적으로 사용할 수 있는 환경을 제공합니다. ex) Web3 게임, 소셜 미디어 dapp, 거래 플랫폼 등
###### 이더리움 네트워크의 확장성 문제를 해결 방법

거래 처리 속도 증가

- **PoS 합의 메커니즘**: Polygon PoS는 작업 증명(PoW)이 아닌 지분 증명(PoS) 합의 메커니즘을 사용합니다. PoW는 많은 연산 자원을 필요로 하여 트랜잭션 처리 속도가 느리고 비용이 높아지는 반면, PoS는 상대적으로 자원이 덜 들고 더 빠른 합의 과정을 가능하게 합니다. 이로 인해 트랜잭션 처리 속도가 빨라지고 수수료가 낮아집니다.
- **Plasma 프레임워크**: Plasma는 많은 트랜잭션을 오프체인에서 처리한 후 최종 결과만 메인체인에 기록합니다. 이 방식은 메인체인의 혼잡을 줄이고 거래 속도를 크게 향상시킵니다.

거래 비용 절감

- **PoS의 비용 효율성**: PoS는 블록 생성과 검증에 필요한 에너지가 적어 비용이 낮습니다. 이는 트랜잭션 수수료를 더 낮게 설정할 수 있음을 의미합니다.
- **Plasma의 오프체인 처리**: Plasma 사이드체인은 많은 트랜잭션을 오프체인에서 처리하여 메인체인의 혼잡을 줄이고, 그 결과 거래 비용을 절감합니다.

네트워크 혼잡 완화

- **병렬 처리**: Polygon PoS는 여러 개의 블록체인을 병렬로 운영하여 트랜잭션을 분산 처리합니다. 이는 메인체인의 혼잡을 줄여주고 네트워크의 전체 처리량을 증가시킵니다.
- **스케일링 솔루션**: Plasma 프레임워크와 PoS 메커니즘을 결합하여 더 많은 트랜잭션을 효율적으로 처리함으로써 네트워크의 혼잡을 완화시킵니다.

확장성 및 유연성

- **EVM 호환성**: Polygon PoS는 Ethereum Virtual Machine(EVM)과 호환되어, Ethereum 상의 기존 dApp(탈중앙화 애플리케이션)을 쉽게 Polygon 네트워크로 이전할 수 있습니다. 이는 개발자들이 Ethereum의 기존 인프라와 도구를 그대로 사용하면서도 확장성을 얻을 수 있게 합니다.
- **다양한 사용 사례 지원**: Polygon PoS는 다양한 블록체인 기반 애플리케이션을 지원할 수 있도록 설계되어 있어, 게임, 금융 서비스, NFT 마켓플레이스 등 다양한 분야에서 활용될 수 있습니다.

이와 같은 기술적 특징들 덕분에 Polygon PoS는 Ethereum 네트워크의 확장성 문제를 효과적으로 해결하여 사용자에게 더 낮은 트랜잭션 수수료와 더 빠른 트랜잭션 처리를 제공할 수 있습니다.


### Polygon zkEVM
#### Architecture
Polygon zkEVM은 zk-rollup 아키텍처를 활용하여 Ethereum 네트워크 상에서 작동하는 L2 솔루션을 제공합니다. zk-rollup 기술은 복잡한 연산을 오프체인에서 수행하면서 암호학적 증명을 생성하고 온체인에 제출할 수 있게 합니다.
#### Consensus Mechanism
다중 코디네이터(sequencers and aggregators 포함)가 특정한 조건이나 허가 없이도 누구나 참여할 수 있는 시스템을 지원하는 consensus contract를 활용하여 오프체인 거래를 일괄 처리하고 검증하여 온체인에 제출합니다. 이 시스템에서는 스테이킹이나 특별한 역할이 요구되지 않습니다.

#### Smart Contract Compatibility
Polygon zkEVM은 EVM 동등성을 목표로 하며, 이는 이더리움 네트워크와 더 높은 호환성을 의미합니다. 결과적으로 Polygon zkEVM을 사용하면 기존의 이더리움 스마트 계약을 어떠한 수정 없이도 네트워크 상에서 원활하게 배포하고 실행할 수 있습니다.
Polygon zkEVM이 이더리움 실행 환경을 완전히 복제하려는 반면, Polygon PoS는 이더리움 smart contract와의 호환성을 제공하는 데 중점을 둔다는 점입니다. 따라서 Polygon zkEVM을 사용하면 L2 솔루션에서도 이더리움 메인넷과 동일한 작업 흐름을 유지할 수 있습니다.
#### Security
Polygon zkEVM은 오프체인 계산의 정확성과 보안을 보장하기 위해 유효성 증명을 온체인에 공개함으로써 이더리움에 의해 보호받습니다.
#### Data Availability
두 가지 옵션을 제공하는 hybrid solution 제공
- Validium: 데이터는 오프체인에 저장되며, 이더리움 메인넷에는 증명만 저장됩니다. 이 방법은 저장 효율성을 향상시킴.
- 중요한 데이터를 선택적으로 온체인에 저장하고 나머지는 오프체인에 유지하는 하이브리드 옵션입니다. 이는 저장 효율성과 빠른 접근 사이의 균형을 제공합니다.
#### Use Cases
Polygon zkEVM은 높은 보안성을 자랑하며, L2 솔루션은 이더리움 메인넷을 활용합니다. 이는 낮은 트랜잭션 슈슈료보다 보안이 우선시되는 yield farming, 고가 DeFi 애플리케이션 등과 같은 사용 사례에 이상적입니다.

Polygon PoS와 Polygon zkEVM 모두 낮은 수수료와 높은 보안성을 제공합니다. 따라서 Polygon zkEVM에서 높은 트랜잭션 볼륨의 dapp을 구축하거나 Polygon PoS에서 안전한 플랫폼을 구축하는 데 제한은 없습니다.
###### 참고
“폴리곤은 웹3 산업의 TCP·IP 프로토콜을 만들고자 하는데, 이는 영지식의 기능을 통해서만 활성화된다”며 이같이 말했다. 영지식이란 대상자에게 본인의 개인정보를 노출하지 않는 것을 의미한다. 이를 증명하는 것을 ‘영지식 증명’이라고 통칭한다.
폴리곤 zkEVM은 이더리움 가상머신(EVM)의 레이어2 네트워크다. 영지식 증명이라는 디지털 서명 체계(암호화 프리미티브)를 사용해 상태 전환을 검증한다.
“폴리곤의 비전은 통합적인 유동성을 통해 무제한의 확장성을 구현하는 것”이라며 “영지식 증명의 힘을 사용해 가스비가 전혀 없는 체인을 출시할 수 있고, 유동성 또한 영지식 증명의 힘을 통해 한곳으로 통합된다”
"오늘날 인터넷이 성장한 것은 어느 정도의 이질성이 허용됨에 따라 다양한 실험과 다양한 네트워크가 구축될 수 있었기 때문이라고 생각한다”고 강조했다. 그러면서 “일반적인 인터넷 커뮤니티처럼 모든 디지털 비지니스가 가스비 없는 인프라 위에 구축되기를 바란다

“독립적이고 자주적이면서 상호 연결돼 있고 결합 가능한 많은 블록체인으로 이루어진 네트워크가 폴리곤의 영지식(ZK) 기술의 힘으로 실현되고 있다. 이 네트워크가 웹3라고 불리는, 가스비 없는 인터넷의 기반이 될 것으로 믿는다.”

 ##### testnet
- Polygon PoS Amoy
- Polygon zkEVM Cardona
- Sepolia

[PoS vs zkEVM](https://moralis.io/whats-the-difference-between-polygon-pos-vs-polygon-zkevm/)
# Layer (폴리곤 레이어)
![[Pasted image 20240627161931.png]]
#### 헤임달(Heimdall) 레이어
 a validator layer
 - 이더리움 메인넷의 스테이킹 컨트랙트 모니터링
- Bor 체인에서 발생하는 모든 트랜잭션과 그로 인해 발생하는 상태 변화 검증
- Bor 체인 상태 체크포인트를 이더리움 메인넷에 커밋
- Tendermint 기반

폴리곤은 레이어1인 이더리움에 폴리곤 네트워크의 일종의 스냅샷인 체크포인트를 제출하고 검증인 셋을 관리하는 등 폴리곤의 심장 역할을 함 <br></br>

#### 보어(Bor) 레이어
 a block producer EVM compatible layer
 - Polygon PoS에서 블록을 생성.
- Bor는 Polygon PoS 네트워크의 블록 생성 노드이자 계층. 
- Go Ethereum을 기반. 
- Bor에서 생성된 블록은 Heimdall 노드에 의해 검증됩니다.

EVM호환 사이드체인 역할을 함
일반적으로 ‘폴리곤을 사용한다’ 라는 것의 의미는 폴리곤의 보어 레이어를 사용한다는 것과 같다.

#### 이더리움 레이어
 A set of contracts on the Ethereum mainnet.

참고 : [[Reorg]]
https://docs.polygon.technology/pos/architecture/
# Validator
### validator 되는 법
- Run a sentry node: A separate machine running a Heimdall node and a Bor node. A sentry node is open to all nodes on the Polygon PoS network.
- Run a validator node: A separate machine running a Heimdall node and a Bor node. A validator node is only open to its sentry node and closed to the rest of the network.
- Stake the MATIC tokens in the staking contracts deployed on the Ethereum mainnet.

running a validator node (sentry + validator) 하여 벨리데이터가 될 자격을 가질 수 있다.
최소 1 MATIC 을 스테이킹 해야함
Keep ETH balance (between 0.5 to 1) on the signer address.

PIP4 governance proposal 시행 후 최소 스테이킹 amount는 10,000 MATIC으로 증가할 것입니다.

##### Technical node operations
노드가 자동으로 수행.
###### Block producer selection:
각 span 마다 블록 생성을 위한 검증자 subnet 선택
span마다 Heimdall에서 블록 생성자 세트를 다시 선택하고 이 정보를 주기적으로 Bor로 전송합니다.
###### Validating blocks on Bor:
Bor 블록 set에 대해, 각 검증자는 해당 블록의 데이터를 독립적으로 읽고 Heimdall에서 데이터를 검증합니다.
###### Checkpoint submission:
각 Heimdall 블록에 대해, 검증자(validator) 중에서 선택된 제안자(proposer), 체크포인트 제안자는 Bor 블록 데이터의 체크포인트를 생성하고 검증한 후, 다른 검증자들에게 동의할 서명된 트랜잭션을 브로드캐스트합니다.
active validator 중 2/3 이상이 체크포인트에 동의하면, 해당 체크포인트가 이더리움 메인넷에 제출됩니다.
###### Sync changes to Polygon staking contracts on Ethereum:
체크포인트 제출 단계 이후, 외부 network 호출이기 때문에 Ethereum에서의 체크포인트 트랜잭션이 확인되지 않거나 Ethereum 네트워크 혼잡 문제로 인해 대기 중인 경우도 있습니다. 이런 경우, 이전 Bor 블록의 스냅샷을 포함하는 다음 체크포인트가 보장되도록 하는 ack/no-ack 프로세스가 있다. 예를 들어, 체크포인트 1이 Bor 블록 1-256을 포함하고 실패한 경우, 다음 체크포인트 2는 Bor 블록 1-512를 포함합니다.
###### State sync from the Ethereum mainnet to Bor:
Contract state는 Ethereum과 Polygon 간에 Bor를 통해 이동할 수 있습니다.
Ethereum의 dApp Contract가 Ethereum의 특별한 Polygon Contract에서 함수를 호출합니다.
해당 이벤트는 Heimdall로 중계되고 그 후에 Bor로 전달됩니다.
Polygon smart contract에서 상태 동기화 트랜잭션이 호출되고, dApp은 Bor 자체에서 함수 호출을 통해 Bor의 값을 가져올 수 있습니다.

Polygon에서 Ethereum으로 상태를 보내는 데에도 비슷한 메커니즘.

[polygon validator](https://docs.polygon.technology/pos/get-started/becoming-a-validator/)

# Transaction
gas estimations and costs of transactions 은 EIP-1559 따름
[Polygon Gas Station V2 Endpoint](https://gasstation.polygon.technology/v2)

# Meta-transactions
메타 트랜잭션은 사용자가 가스 요금을 지불하지 않고도 블록체인과 상호 작용할 수 있게 해주는 기술.
메타 트랜잭션을 통해 사용자는 블록체인 네트워크에서 거래 수수료를 지불하기 위해 토큰을 보유할 필요가 없다. 이는 트랜잭션의 발송자와 가스 요금을 지불하는 주체를 분리함으로써 수행된다.
사용자가 어떤 작업을 수행하려는 의도를 가진 트랜잭션 요청을 서명하면, 이 서명된 요청을 릴레이어(relayer)에게 전달합니다. 릴레이어는 이 요청을 실제 트랜잭션으로 변환하고 가스 요금을 지불하여 블록체인에 트랜잭션을 제출합니다.
### 메타 트랜잭션의 작동 원리

1. **사용자 서명**: 사용자가 트랜잭션 요청에 서명합니다. 이 서명된 요청은 사용자가 수행하려는 작업(트랜잭션 파라미터)을 포함합니다.
2. **릴레이어**: 이 서명된 요청을 릴레이어에게 전달합니다. 릴레이어는 요청을 검증하고, 트랜잭션을 실제 블록체인 트랜잭션으로 변환합니다.
3. **가스 요금 지불**: 릴레이어가 가스 요금을 지불하고 트랜잭션을 블록체인 네트워크에 브로드캐스트합니다.
4. **계약 실행**: 블록체인 계약이 트랜잭션을 실행합니다.
### 메타 트랜잭션의 사용 사례

1. **투표**: 사용자가 온체인 거버넌스에 참여하여 특정 결과에 투표하려고 할 때, 사용자는 전통적으로 가스 요금을 지불해야 합니다. 그러나 메타 트랜잭션을 사용하면 사용자는 단순히 투표 정보를 서명하고 이를 릴레이어에게 전달하면 됩니다. 릴레이어가 가스 요금을 지불하고 트랜잭션을 제출하여 사용자의 투표가 실행됩니다.
2. **dApp 상호작용**: dApp에서 메타 트랜잭션을 사용하면 사용자는 가스 요금을 지불하지 않고도 여러 번 트랜잭션을 생성할 수 있습니다. 이는 dApp의 확장성과 사용성을 크게 향상시킵니다.

# Polygon 2.0

2023년 6월 대규모 업데이트

> [!quote] “무한한 확장성(Unlimited Scalability)”과 “통합(Unified)”된 환경
 
인터넷상의 “가치 계층(Value Layer)”이 되어 블록체인을 사용하는 유저 누구나 가치를 생성하고 교환, 그리고 프로그래밍할 수 있는 프로토콜로 거듭나겠다
![[Pasted image 20240628121925.png]]
#### 폴리곤 솔루션
- Polygon PoS: 폴리곤의 지분증명 네트워크
- Polygon zkEVM: EVM 호환성을 강조한 ZK 롤업 프로젝트
- Polygon Miden: STARK 기술 기반 ZK 롤업 프로젝트로 EVM 호환성보다는 ZK 친화적 디자인에 방점을 둠
- Polygon CDK: 폴리곤의 ZK 네트워크에 커스터마이징 앱체인을 만들 수 있도록 지원하는 프로그램
- Polygon ID: ZK 증명을 활용한 디지털 신원 인증 서비스
## POL (Polygon Ecosystem Token)
MATIC 토큰은 공급량 대부분이 유통되었기 때문에 다가올 ZK L2 경쟁에서 폴리곤이 생태계 빌더들을 지속적으로 모집하고 지원하는 데에 분명히 한계가 존재한다.

특히 ZK 롤업 프로젝트 중 괄목할만한 성장 속도를 보여주고 있는 zkSync, Starknet 등이 새로 토큰을 론칭할 예정인 것을 감안한다면, 인센티브 측면에서 경쟁력을 확보하기 위해 한계에 다다른 토크노믹스를 개선하는 작업이 불가피했다. 크게 밸리데이터 보상 물량을 확보하고 트레저리의 확충을 통해 지속적인 생태계 성장을 도모하는 내용을 담고 있는 토크노믹스 개선안은 다음과 같다.

- 기존의 MATIC 토큰을 POL 토큰으로 1:1 마이그레이션
- POL은 ▲스테이킹 자본 ▲밸리데이터 보상 ▲거버넌스 투표권의 유틸리티를 가짐
- 토큰 하드캡을 없애고 Inflationary 자산으로 변모
- POL 토크노믹스 백서에서는 연간 2%의 토큰 인플레이션을 제안  
    - 밸리데이터 보상으로 1%, 생태계 지원을 위한 커뮤니티 트레저리로 1%를 할당  
    - 인플레이션율은 토크노믹스 업데이트 이후 10년간 변경할 수 없고, 그 이후 커뮤니티 거버넌스를 통해 인플레이션 감소 결정이 가능  
    - 다만, 각 할당량에 대해 1% 이상으로 증가시키는 결정은 불가
    
멀티 체인 토큰

참고 : https://research.despread.io/ko/reports-polygon2/
[polygon2.0](https://polygon.technology/blog/polygon-2-0-protocol-vision-and-architecture)
## faucet
Polygon
https://faucet.polygon.technology/
ERC-20
https://faucets.chain.link/polygon-amoy

![[Pasted image 20240613161440.png]]