# Klaytn
Layer-1 (L1) network to Ethereum. 
Consensus(합의방식) : Proof-of-Stake (PoS) 
4,000 TPS (Real-Time TPS : 12.03 tx/s)
Block Time(블록생성시간) : 1초
Time-to-finality : Instant Finality(즉각적 완결성)

# 클레이튼 네트워크 토폴로지
Klaytn에는 CN(컨센서스 노드), PN(프록시 노드), EN(엔드포인트 노드)의 세 가지 유형의 노드가 있다. 
- Consensus Node (CN): CN은 CCO(코어 셀 운영자)가 관리하며 트랜잭션의 유효성을 검사하고 새로운 블록을 생성하는 역할을 담당합니다. 이러한 블록은 네트워크의 모든 노드에 의해 검증됩니다.
- Proxy Node (PN): PN은 CN과 EN 사이에서 트랜잭션을 중계합니다.
- Endpoint Node (EN): EN은 클레이튼 네트워크에 대한 퍼블릭 액세스를 제공합니다.


# 합의 메커니즘
#### Istanbul BFT(실용적인 비잔틴 장애 허용"([PBFT, Practical Byzantine Fault Tolerance](http://www.pmg.csail.mit.edu/papers/bft-tocs.pdf)))

각 블록마다 검증과 합의가 이루어지기 때문에 포크가 없고 합의가 이루어지는 즉시 블록의 완결성이 보장됨.
또한 BFT 알고리즘에서 통신량이 증가하는 문제는 무작위로 선정된 '위원회'를 활용하여 해결. 
CN들은 집합적으로 '위원회'를 구성하고 블록 생성 시마다 VRF(Verifiable Random Function, 검증 가능한 랜덤 함수)를 사용하여 일부가 '위원회'의 멤버로 선정된다.
합의 메시지는 위원회 구성원 간에만 주고받기 때문에 총 CN 수가 증가하더라도 통신량은 설계된 수준 이하로 제한될 수 있다.
현재 클레이튼 메인넷 Cypress는 1초 블록 생성 간격으로 초당 4,000개의 트랜잭션을 처리할 수 있는 높은 처리량을 제공합니다. 현재 50개 이상의 컨센서스 노드가 CNN(합의 노드 네트워크)에 참여할 수 있으며, 클레이튼이 알고리즘을 지속적으로 최적화함에 따라 그 수는 계속 늘어날 것.
# Klay
유통 공급량 : 34억 9천만
단위 :
- `peb`는 가장 작은 화폐 단위입니다.
- `ston`은 `Gpeb`의 별칭으로 편의상 도입되었습니다.
- `KLAY`는 10^18peb입니다.
# Transaction Fee

>[!info] Transaction fee := (Gas used) x (GasPrice)

## 필요한 Gas 양(gasLimit) 
= IntrinsicGas + ExecutionGas
###### IntrinsicGas
트랜잭션의 데이터 크기와 같은 트랜잭션의 구성에 따라 정적으로 부과되는 gas.
[Intrinsic Gas](https://docs.klaytn.foundation/ko/docs/learn/transaction-fees/intrinsic-gas/).
###### ExecutionGas
contract 실행을 위해 동적으로 계산되는 gas 
[Execution Gas](https://docs.klaytn.foundation/ko/docs/learn/transaction-fees/execution-gas/).

## Gas price ( = baseFee)
### Dynamic Fee Policy
동적 가스비 정책 [KIP-71](https://github.com/klaytn/kips/blob/master/KIPs/kip-71.md)

동적 가스비 정책에서의 거래(transaction)는 네트워크의 포화 상태에 따라 동적으로 조절되는 `base_fee` 로 구성됩니다. 네트워크의 포화도는 Klaytn에서 생성되는 블록(Block)의 사용량인 `gas_used` 를 통해 측정됩니다. `base_fee` 는 매 블록 변경됩니다.

7가지 파라미터가 `base fee(gas fee)`에 영향을 미칩니다

1. PREVIOUS_BASE_FEE: 이전 블록의 기본 수수료
2. GAS_USED_FOR_THE_PREVIOUS_BLOCK: 이전 블록의 모든 트랜잭션을 처리하는 데 사용된 gas
3. GAS_TARGET: 기본 수수료의 증감을 결정하는 gas 금액(현재 3,000만)
4. MAX_BLOCK_GAS_USED_FOR_BASE_FEE: 최대 기본 수수료 변경율을 적용하는 암시적 블록 gas 한도(현재 6천만)
5. BASE_FEE_DELTA_REDUCING_DENOMINATOR: 최대 기본 수수료 변경을 블록당 5%로 설정하는 값 (현재 20, 추후 거버넌스에 의해 변경 가능)
6. UPPER_BOUND_BASE_FEE: 기본 수수료의 최대값 (현재 750 ston, 추후 거버넌스에 의해 변경될 수 있음)
7. LOWER_BOUND_BASE_FEE: 기본 수수료의 최소값 (현재 25 ston, 추후 거버넌스에 의해 변경될 수 있음)

- 한 블록에 들어간 트랜잭션들은 동일한 블록 가스비(`baseFee`)로 트랜잭션 비용을 계산하며, 블록 가스비 이상의 가스비를 설정한 트랜잭션만 블록에 담길 수 있음
- 블록 가스비는 이전 블록의 가스 사용량에 따라 자동 증가/감소하며 최대 변동폭은 5%
- 블록 가스비는 25 ston ~ 750 ston 이며, 이 범위는 Governance 기능으로 변경 가능
- 매 블록에서 사용된 트랜잭션 비용의 절반은 자동 소각

>[!note] 이더리움과 차이점
>이더리움의 EIP-1559와 클레이튼을 차별화하는 중요한 특징은 Tip이 존재하지 않는다.
>클레이튼은 트랜잭션에 대해 선착순(FCFS) 원칙을 따릅니다. 
>Tip 이 존재하지 않는 이유는? 
>기존 토큰 전송 등의 UX 저하를 최소화 하면서도, 가스비의 예측 가능성을 일부분 담보 하고자 함.
>
>base_fee 에 대한 최저값과 최대값이 존재한다 .
>각 블록의 트랜잭션 수수료의 절반이 소각됩니다(BURN_RATIO = 0.5, 거버넌스에 의해 변경 불가).

[transaction-fee docs](https://docs.klaytn.foundation/ko/docs/learn/transaction-fees/)
[forum 설명 KR](https://forum.klaytn.foundation/t/kr-new-transaction-fee-mechanism/4843)

explorer transaction 확인

gas price: 트랜잭션에 사용될 것으로 예상되는 gas의 가격을 KLAY로 표현한 값  
effective gas price: 트랜잭션에서 사용된 실제 gas의 가격을 KLAY로 표현한 값  
gas used: 트랜잭션에 실제로 사용된 gas의 양  
gas limit: 트랜잭션에서 사용될 수 있는 gas의 최대 양  
tx fee: 트랜잭션에서 사용된 수수료(사용된 gas의 양 * 실제 gas 가격)  
burnt fees: 트랜잭션에서 사용된 수수료 중 일부를 클레이튼 전체 생태계를 위해서 소각된 KLAY의 양

# Klaytn Token Standards
- KIP7 (Equivalence of Ethereum ERC20): [https://kips.klaytn.com/KIPs/kip-7](https://kips.klaytn.com/KIPs/kip-7)
- KIP17 (Equivalence of Ethereum ERC721): [https://kips.klaytn.com/KIPs/kip-17](https://kips.klaytn.com/KIPs/kip-17)
- KIP37 (Equivalence of Ethereum ERC1155): [http://kips.klaytn.com/KIPs/kip-37](http://kips.klaytn.com/KIPs/kip-37)

# 수수료 위임
## **기본 수수료 위임(Basic Fee Delegation)**: 
    - 사용자가 트랜잭션을 제출할 때 트랜잭션 수수료를 제3자가 대신 지불할 수 있습니다.
    - 이를 통해 사용자는 트랜잭션 수수료를 신경 쓰지 않고 서비스를 사용할 수 있습니다.
## **부분 수수료 위임(Partial Fee Delegation)**: 
    - 트랜잭션 수수료의 일부를 사용자가, 나머지 부분을 제3자가 지불할 수 있습니다.
    - 이는 사용자가 일부 책임을 가지면서도 부담을 덜 수 있도록 합니다.

### 수수료 위임의 동작 방식
수수료 위임은 주로 다음과 같은 과정으로 이루어집니다:

1. **트랜잭션 생성**:
    
    - 사용자가 트랜잭션을 생성합니다. 이 트랜잭션은 일반 트랜잭션과 동일하지만, 수수료 위임에 대한 정보를 추가로 포함할 수 있습니다.
2. **수수료 위임 요청**:
    
    - 사용자는 트랜잭션에 수수료 위임을 요청하는 플래그를 설정합니다. 이를 통해 수수료를 대신 지불해줄 대납자(delegator)를 지정할 수 있습니다.
3. **트랜잭션 서명 및 전송**:
    
    - 사용자는 트랜잭션을 서명하고, 이를 네트워크에 전송합니다. 이때, 트랜잭션에는 수수료 대납에 대한 정보가 포함됩니다.
4. **수수료 대납자의 승인**:
    
    - 수수료 대납자는 해당 트랜잭션을 확인하고, 트랜잭션 수수료를 대신 지불합니다. 이는 일반적으로 스마트 계약이나 미들웨어를 통해 자동으로 이루어질 수 있습니다.
5. **트랜잭션 처리**:
    
    - 클레이튼 네트워크는 트랜잭션을 처리하고, 결과를 기록합니다. 수수료는 대납자의 계정에서 차감됩니다.

### 장점

1. **사용자 경험 개선**:
    
    - 사용자가 트랜잭션 수수료를 직접 지불하지 않아도 되므로, 블록체인 애플리케이션 사용이 더욱 간편해집니다. 이는 블록체인 기술의 대중화를 촉진합니다.
2. **서비스 제공자의 유연성**:
    
    - 서비스 제공자는 사용자가 부담하는 수수료를 대신 지불함으로써, 더 많은 사용자를 유치하고 서비스의 접근성을 높일 수 있습니다.
3. **비즈니스 모델 다양화**:
    
    - 수수료 대납 기능을 통해 다양한 비즈니스 모델을 구현할 수 있습니다. 예를 들어, 광고 기반 수익 모델이나 프리미엄 서비스 모델에서 수수료 대납을 활용할 수 있습니다.

### 클레이튼 수수료 위임의 활용 사례

- **게임 및 엔터테인먼트**:
    
    - 게임 내에서 발생하는 트랜잭션 수수료를 게임 개발사가 대납하여, 사용자가 게임을 즐기는 데 집중할 수 있도록 합니다.
- **소셜 네트워크**:
    
    - 소셜 플랫폼에서 사용자가 게시물을 올리거나 상호작용할 때 발생하는 수수료를 플랫폼이 대납함으로써, 사용자 참여를 유도합니다.
- **금융 서비스**:
    
    - 디파이(DeFi) 플랫폼에서 거래 수수료를 플랫폼이 대신 지불하여, 사용자들이 더 활발히 거래할 수 있도록 지원합니다.

클레이튼의 수수료 위임 기능은 블록체인 기술을 실생활에서 더욱 쉽게 사용할 수 있도록 하는 중요한 도구입니다. 이를 통해 클레이튼은 다양한 상용 서비스와 애플리케이션에서의 활용을 촉진하고, 블록체인 기술의 대중화를 앞당기고 있습니다.
# 참고 링크
docs: 
https://docs.klaytn.foundation/ko/
https://docs.kaia.io/ (new version)