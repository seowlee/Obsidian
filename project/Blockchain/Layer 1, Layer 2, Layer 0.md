# Layer 1 (L1)
블록체인에서 가장 기본이 되는 네트워크
다른 블록체인의 영향을 받지 않는 독립적인 네트워크
각 네트워크에서 트랜잭션을 생성, 처리

# Layer 2
다른 블록체인(L1) 위에 구축된 네트워크
실제 트랜잭션은 L2에서 생성하고 처리한 뒤, 이를 압축한 값만 메인 체인인 L1에 기록. 
따라서 L1에서 모든 트랜잭션을 처리하는 것보다 더 많은 양의 데이터를 빠르게 처리할 수 있다.

레이어2 솔루션은 이더리움 메인넷의 확장성 문제를 해결하기 위해 설계된 별도의 레이어다. 
초당 처리결제수(TPS)가 15로 한정된 이더리움 네트워크에서 L2를 사용할 경우 TPS가 수천에서 수만까지 증가할 수 있다. L2는 이처럼 블록체인의 보안성과 데이터 가용성을 L1(레이어1)에 위탁하는 대신 최대한 빠르고 저렴하게 트랜젝션을 실행하는 것을 목표로 한다. 

스마트 컨트랙트 브릿지로 L1과 연결된 실행(execution) 레이어.

## Layer 2 종류
구조와 암호화 방식에 따라 나뉨
![[Pasted image 20240617103214.png]]
- 롤업(옵티미스틱 롤업, ZK롤업),
- 발리디움,
- 플라즈마,
- 스테이트 채널,
- 사이드체인
### 1. 사이드체인 (Sidechain)

사이드체인은 L1(이더리움)과 2WP(2-way peg)로 연결된 블록체인이다. 2-way peg는 L1에서 사이드체인으로 자산을 보내기 위한 방법인데, 정확히 말하면 L1에 있는 코인을 동결시킨 뒤 같은 양의 코인을 사이드체인에서 사용할 수 있게 해주는 것이다. 사이드체인은 별도의 컨센서스 알고리즘, 블록 생성 규칙, 그리고 검증자 노드를 보유하고 있어 보안이 취약할 수 있다. 같은 이유로 사이드체인을 L2 솔루션으로 취급하지 않는 사람들도 많다. 사이드체인은 EVM 호환이 가능하며 목적에 따라 다양한 방법으로 설계할 수 있다. 대표적인 사이드체인은 [Polygon](https://xangle.io/project/MATIC/key-info/?id=credibility-rating), [Skale Network](https://xangle.io/project/SKL/key-info/?id=credibility-rating), xDai 등이 있다. (해당 프로젝트에 대한 자세한 내용은 XCR 리포트 참고).

### 2. 스테이트 채널 (State Channel)

스테이트 채널은 사용자들이 2번의 온체인 트랜젝션으로 n번의 거래를 처리할 수 있게 해주는 L2 솔루션이다. 스테이트 채널을 사용하고자 하는 유저는 스테이트 채널을 열 때 (개시할 때) 거래하고자 하는 대금(ETH)를 채널에 전송하고 트랜젝션 수수료를 한 번 지불해야 한다. 채널을 연 이후에는 닫을 때까지 채널에 참가한 다른 사용자들과 n번의 거래를 할 수 있다. 이론상으로는 무한정 거래할 수 있으나, 채널이 닫힐 때까지 자금을 받지 못한다. 거래가 끝난 이후에는 각자 프라이빗 키로 디지털 서명을 한 뒤 트랜젝션 수수료를 다시 한 번 내고 (총 2번) 채널을 닫으면 된다. 만약 채널에 참가한 어떤 유저가 불순한 목적을 가지고 채널을 닫지 않는다면, 상대방은 '회수 기간' (withdrawal period, 주로 7일)을 신청할 수 있다. 회수 기간이 끝날 때까지 유저가 채널을 닫기 위한 서명을 제공하지 않는다면 여태까지 있던 모든 거래가 취소되고 모든 자금이 상대방에게로 돌아간다.

스테이트 채널은 장기간 지속되는 온체인 자금 거래에 있어 매우 효과적이지만, 채널에 참가하지 않은 대상과 거래를 할 수 없으며 단순 자금 거래 외 다른 목적으로 사용하고자 하면 막대한 양의 자금을 채널에 묶어놔야 한다는 단점이 있다.

대표적인 스테이트 채널은 Lightning Network, Celer, Raiden 등이 있다.

### 3. 롤업(Rollups), 발리디움(Validium), 플라즈마(Plasma)

나머지 L2는 **암호화 증명(cryptographic proof) 방식**과 **데이터 가용성 (data availability)의 오프체인 / 오프체인 여부**에 따라 종류를 구분할 수 있다. 아래 Avihu Levy (Head of Product at Starkware)가 그린 2x2 매트릭스를 참고하면 이해하기 편할 것이다.

![레이어2 솔루션, 레이어2, 레이어2 종류](https://d1kkf5jqktb75.cloudfront.net/images/content/common/Frame_283-2d9e0d92-b8c0-4716-8941-eda6f55fbf92.png)

출처: [Avihu Levy](https://twitter.com/avihu28/status/1267446095556345857)

#### 암호화 증명 방식 (Computation)

암호화 증명 방식은 유효성 증명과 사기 증명으로 구분할 수 있다.

**유효성 증명 (Validity Proof)**

- 데이터를 직접 보여주지 않고도 검증자(verifier)에게 해당 정보가 이상 없음을 수학적으로 증명하는 영지식 증명 (zero knowledge proof, 혹은 ZKP)을 활용한 암호화 방식
- 유효성 증명은 유저들의 트랜젝션을 포함하고 있는 state가 최신화될 때마다 해당 데이터가 이상 없음을 증명하는 수학적 증거를 제시한다
- 즉, 유효성 증명은 L2에 포함되는 모든 트랜젝션들이 정확하다는 것을 수학적으로 검증하기 때문에 데이터에 대한 신뢰도가 높다
- 순자의 성악설과 유사하다 (=인간의 본성은 악하기 때문에 모든 트랜젝션을 하나씩 직접 검증해야 한다)
- ZKP는 크게 ZK-SNARK와 ZK-STARK으로 나뉘며, 사기 증명 대비 기술적으로 매우 복잡하고 구현하기 어렵다

**사기 증명 (Fraud Proof)**

- 사기 증명은 트랜젝션이 이상 없다는 것을 기본 전제로 하여 별도의 검증 절차를 거치지 않는다
- 다만, state transition이 일어나기 전, 부정확한 (혹은 거짓된) 트랜젝션에 대해 이의를 제기할 수 있는 DTD (dispute time delay) 유예 기간이 주어진다
- 만약 해당 트랜젝션이 부정확하다는 것이 밝혀지면 L2는 기존 state로 롤백하고 해당 트랜젝션을 통과시킨 검증자를 처벌한다
- 맹자의 성선설과 유사하다 (=인간의 본성은 선하기 때문에 거짓된 트랜젝션이 없을 것이다)
- 기술적으로 구현하기가 쉽다는 장점이 있으나, 유예 기간 동안에는 사용자들이 자신의 자금을 출금하지 못한다는 단점이 있다 (일반적으로 7~14일 소요)

#### 데이터 저장 방식 (Data Storage)

- 온체인: 모든 트랜젝션의 calldata (스마트 컨트랙트 호출, 토큰 거래, 디지털 서명 데이터를 저장하고 있는 위치)를 포함한 state data가 블록체인(온체인)에 저장 및 기록되어 블록체인 상에서 데이터를 확인하고 검증할 수 있음
- 오프체인: calldata 및 state data를 블록체인 외부(오프체인)에 처리 및 기록하는 방식. 온체인 저장에 비해 확장성 측면에서 더 우수하다는 장점이 있으나, 보안성이 떨어지고 중앙화된 방식이라는 단점 존재

#### 3-1. 롤업이란? (Rollups)

롤업이란 L2에서 수많은 트랜젝션을 실행 혹은 처리하고 그 결과값들을 배치(batch)로 롤업하여(묶은 뒤) L1에 저장하는 기술이다 (아래 그림 참고). 비탈릭 부테린은 "이더리움 생태계는 중장기적 확장성 개선 전략 차원에서 롤업에 올인하게 될 것"이라고 언급한 바 있을 정도로 롤업은 향후 이더리움의 생태계에 핵심적인 역할을 하게 될 것으로 예상되며 그만큼 업계 내에서 핫한 L2 솔루션 중 하나이다.

롤업은 모두 온체인 저장방식이지만, 사용하는 암호화 증명(cryptographic proof) 방식에 따라 옵티미스틱 롤업(optimistic rollup)과 ZK롤업으로 나뉜다.

###### 롤업의 작동 방식 (state root 업데이트 전)

![롤업이란, 롤업 원리, 레이어 2 솔루션](https://d1kkf5jqktb75.cloudfront.net/images/content/common/2-b88867de-abcd-4b9c-88f2-2e4dd14260e2.JPG)

###### 롤업의 작동 방식 (state root 업데이트 후)

![롤업 원리, 롤업이란, 레이어 2 솔루션, 레이어 2](https://d1kkf5jqktb75.cloudfront.net/images/content/common/3-5963711b-b7bf-464f-9fbc-9b7d96a23fe9.jpg)

출처: [비탈릭 부테린](https://vitalik.ca/general/2021/01/05/rollup.html)

#### 3-1-1. 옵티미스틱 롤업 (Optimistic Rollup, 혹은 OR) 이란?

![옵티미스틱 롤업, 옵티미스틱 롤업이란, 레이어 2 솔루션, 레옵티미스틱 롤업, 레이어 2 솔루션, 레이어 2, 롤업이어 2, 롤업](https://d1kkf5jqktb75.cloudfront.net/images/content/common/Frame_163-1-22f7afd6-978b-439f-bcd6-512c2379c73c.png)

출처: [Avihu Levy](https://twitter.com/avihu28/status/1267446095556345857)

  
OR은 사기 증명 기반의 롤업을 뜻한다. OR은 기본적으로 모든 트랜젝션이 이상 없다고 가정하기 때문에 L2 관리자(operator)는 별도의 검증 절차 없이 merkle root에 유저들의 트랜젝션을 담아 state root를 업데이트한다. 다만, 최신화된 트랜젝션 데이터를 담은 state root를 L1에 반영하기 전 DTD 유예 기간이 주어지며, 이때 관리자와 유저들은 잘못된 트랜젝션이 있는 지 검증할 수 있다 (DTD 기간에는 어떠한 자금 회수도 불가능하다). 만약 잘못된 트랜젝션이 적발된 경우 OR 체인은 기존 블록으로 롤백되고 해당 트랜젝션을 반영한 관리자들을 처벌한다 (주로 관리자 자격을 얻기 위해 스테이킹한 코인을 회수함).

OR의 최대 장점은 구축하기가 비교적 쉽고 EVM호환이 가능하다는 점이다. 스마트 컨트랙트 호출과 같이 EVM에서 가능한 건 OR의 VM에서도 모두 가능하다.

대표적인 OR 프로젝트로 Arbitrum, Optimism, 그리고 Boba Network 등이 있다. 현재 TVL 기준 가장 잘나가는 OR은 Arbitrum($2.45B), Boba Network($874M), Optimism($423M) 순이며 자세한 정보는 [L2Beat](https://l2beat.com/)에서 확인할 수 있다. 흥미로운 점은, Arbitrum로 사용자들이 몰리니까 현재 Arbitrum의 가스비가 약 2.5 달러로 꽤나 비싸졌다. L2별 가스비는 [L2fees](https://l2fees.info/)에서 확인할 수 있다.

#### 3-1-2. ZK롤업이란? (zkRollup)

![ZK 롤업, ZK 롤업이란, 레이어 2 솔루션, 레이어 2, 롤업](https://d1kkf5jqktb75.cloudfront.net/images/content/common/Frame_163-2-a0e5cab6-d29b-465b-aa29-830a7da9cd9d.png)

출처: [Avihu Levy](https://twitter.com/avihu28/status/1267446095556345857)

  
ZK롤업이란 유효성 증명, 더 구체적으로는 영지식 증명 (ZKP)을 사용하는 롤업이다. ZK롤업내 모든 배치는 ZK-STARK/ZK-SNARK 기반의 ZKP를 포함하고 있어 트랜젝션에 대한 검증이 완료된 상태로 state root가 L1에 반영이 된다. 따라서 ZK롤업은 신뢰도가 매우 높은 편이며, OR와 달리 DTD도 없어 자금 인출이 자유로운 편이다. 무엇보다 OR과 달리 트랜젝션이 아무리 몰려도 가스비가 비싸지지 않는다. 

ZK롤업은 거의 모든 면에서 OR 대비 우수하나, 기술 자체가 너무 복잡하고 개념이 출시된 지 얼마 되지 않아 ZK롤업을 구축하기가 매우 어렵다는 단점이 있다. 또한, 여태까지 출시된 ZK롤업은 모두 EVM 호환이 되지 않아 사용성이 제한적이었지만, 최근에 Starkware, Matter Labs, 그리고 Polygon이 EVM호환이 가능한 ZK롤업 개발에 성공했다는 소식이 들리면서 업계를 떠들썩거리게 했다

EVM호환이 가능한 ZK-STARK기반의 ZK롤업은 L2의 **Holy Grail** 같은 존재다. 그만큼 L2 중 가장 유망하고 기술적 완성도가 높은 솔루션으로 평가받고 있다. 수많은 디앱들이 EVM호환 가능한 ZK롤업이 출시되기만을 기다리고 있다.

대표적인 ZK롤업 솔루션으로는 Starkware의 StarkNet(+Cairo) , Matter Labs의 zkSync2.0 (+Zinc), 그리고 Hermez Network를 인수하여 탄생시킨 Polygon의 Miden 등이 있다. 아쉽지만 Starkware와 Matter Labs는 아직 토큰 출시를 하지 않았다.

###### ZK-SNARK vs ZK-STARK 비교

![ZK 롤업, ZK 롤업 비교](https://d1kkf5jqktb75.cloudfront.net/images/content/common/Frame_282-399f42d7-253f-4b4e-b2f4-d7bb840cdb10.png)

출처 Xangle.io, Consensys

###### ZK-SNARK vs ZK-STARK 검증 시간 및 통신 복잡성 비교

![ZK 롤업, 레이어 2 솔루션, 레이어 2, 롤업](https://d1kkf5jqktb75.cloudfront.net/images/content/common/242-6d1ed67a-613a-47a1-8e6e-df73fa7739b1.png)

출처: [Eli Ben-Sasson](https://eprint.iacr.org/2018/046.pdf)

#### 롤업 비교

![롤업 솔루션, 레이어 2 솔루션, 레이어 2, 롤업](https://d1kkf5jqktb75.cloudfront.net/images/content/common/Frame_281-763283f1-f8f0-4d0a-a1b8-84c792397df6.png)

출처: [비탈릭 부테린](https://vitalik.ca/general/2021/01/05/rollup.html)

#### 3-2. 플라즈마 (Plasma)

![플라즈마, 레이어 2 솔루션, 레이어 2, 롤업](https://d1kkf5jqktb75.cloudfront.net/images/content/common/Frame_163-ac27fe35-c52f-4a77-9cee-e531e740db68.png)

출처: [Avihu Levy](https://twitter.com/avihu28/status/1267446095556345857)

  
플라즈마란 한마디로 비수탁(non-custodial) 사이드체인이다. 즉, 사이드체인에 문제가 발생 (예:해킹, 검증자의 악의적 행동, 등등)하면 자금을 회수하지 못할 가능성이 높지만 플라즈마 체인에 문제가 생기면 유저들은 안전하게 자금을 L1에 이동시킬 수 있다. 다만, 사기 증명 방식이라 자금 회수에 시간이 다소 걸리고 (일반적으로 7~14일), L1에 트랜젝션 수수료를 제공해야 한다. 따라서 플라즈마는 사이드체인에 비해 보안이 더 우수하나, 그만큼 플라즈마를 구축하는 것이 기술적으로 더 어렵다. 플라즈마는 크게 Plasma Cash, MVP, Plasma Debit, Plasma Prime 총 4 종류로 나뉜다. 대표적인 플라즈마는 OMG Network과 Gluon이 있다.

#### 3-3. 발리디움 (Validium)

![발리디움, 레이어 2 솔루션, 레이어 2, 롤업](https://d1kkf5jqktb75.cloudfront.net/images/content/common/Frame_163-3-9ad01579-b1ec-42b0-b382-c7448cbed6db.png)

출처: [Avihu Levy](https://twitter.com/avihu28/status/1267446095556345857)

  
발리디움은 ZK롤업과 거의 동일하나 오프체인 저장 방식이라는 차이가 있다. 따라서 발리디움은 진정한 의미의 L2로 보기에는 조금 어렵다. 발리디움은 이더리움의 보안성을 활용하지 않기 때문에 ZK롤업 대비 탈중앙화 및 보안성이 열위하다는 단점이 있다 (그래도 ZK롤업과 같이 배치마다 ZKP가 포함되어 있어 왠만한 사이드체인보다는 보안성이 높다).

이러한 단점을 해결하기 위해 발리디움 솔루션을 제공하는 Starkware같은 경우 트랜젝션의 신뢰성을 보증하는 7개 주체 (Starkware, Consensys, Infura, Nethermind, Iqlusion, Cephalopod, DeversiFi)로 구성된 DAC (Data Availability Committee)를 감시자로 내세워 안전장치를 마련하였다. 발리디움의 최대 장점은 확장성이 매우 우수하다는 점이다. 현재 ZK롤업의 최대 TPS (transaction per second, 초당 거래 횟수)이 2,000~3,000 수준이라면 발리디움은 9,000까지 처리가 가능하다.

#### 3.4. 볼리션 (Volition = zkRollups + Validium)

볼리션이란 ZK롤업과 발리디움을 둘 다 제공해주는 L2솔루션이다. 사용자들은 입맛에 맞게 둘 중 하나를 선택할 수 있다. L2시장에서 선두를 달리고 있는 StarkNet과 zkSync2.0(zkPorter)이 볼리션 구조를 띄고 있어 볼리션이 ZK롤업 시장의 표준이 될 가능성이 높다. Immutable X, Sorare, 그리고 DeversiFi가 현재 Starkware의 볼리션을 사용하고 있다.

## 레이어 2 장단점 비교

마지막으로, 각 L2별로 보안, 퍼포먼스, 사용성, 기타 등의 장단점을 간략하게 비교해봤다.

초록색은 강점, 빨간색은 단점으로 이해하면 된다.

자세히 보면 ZK롤업은 빨간색 칸이 단 한 개도 없다는 것을 확인할 수 있는데, L2중에서 가장 유망한 이유다. 비탈릭 부테린조차 이러한 장점들 때문에 이더리움의 L2로 ZK롤업을 좋게 보고 있는 상황이다.

![레이어 2 장단점, 롤업 솔루션, 레이어 2, 레이어2 솔루션](https://d1kkf5jqktb75.cloudfront.net/images/content/common/Frame_178_1-71b354bc-453e-429a-96e1-e469b807150e.png)

참고 : https://xangle.io/research/detail/453
