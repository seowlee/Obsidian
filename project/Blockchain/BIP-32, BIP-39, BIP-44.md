#### What is BIP?

BIP stands for Bitcoin Improvement Proposal.

  

#### BIP32

hierarchical deterministic wallets (or "HD Wallets") 을 설명함. HD Wallet에 대한 표준

하나의 마스터 키로부터 생성된 자식 키를 위한 계층 구조를 제공.

각 계층은 인덱스(index) 값을 가지며, 하나의 마스터 키에서 무한히 많은 계층을 생성할 수 있다. 즉, 무수히 많은 지갑 주소를 생성할 수 있다. 

![](https://blog.kakaocdn.net/dn/v2eri/btsHb4Bdw0z/VqsXgLtPKkXjfdHQTlj4E1/img.png)

bip32: [https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)

  

  

#### BIP39

deterministic wallet 생성 시 기억하기 쉽게 니모닉 코드/문장의 구현을 설명함.

mnemonic을 생성하고, 이를 binary seed로 변환, 마스터 키를 만드는 데 사용된다.

BIP39는 128-256비트의 엔트로피(entropy)를 생성하고, 엔트로피로부터 12~24개의 단어로 이루어진 니모닉 문구를 생성하는 표준 방식을 제공

니모닉을 통해 마스터 키를 백업하고 복원할 수 있다

  

bip39: [http://bip39: https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki](http://bip39:%20https//github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)

####   

#### BIP44

  

BIP32 와 BIP43 을 기반으로 정의됨 (BIP43 은 BIP32에 purpose 부여)

마스터 키(Master Key)에서 파생된 계정(Account), 각 계정에서 파생된 외부(External) 및 내부(Internal) 체인, 그리고 각 체인에서 파생된 주소(Address)를 사용하여 지갑 주소를 생성

  

path levels

m / purpose' / coin_type' / account' / change / address_index

- m: 마스터 키(Master Key)
- purpose': 고정값 44로 설정
- coin_type': 코인 타입(예: 비트코인, 이더리움 등)에 해당하는 고유한 값으로 설정
- account': 계정(Account) 번호
- change: 외부(External) 및 내부(Internal) 체인 중 하나를 선택(0:외부 체인, 1:내부 체인)
- address_index: 체인에서 파생된 지갑 주소의 인덱스

내부 체인은 1)해당 계정에서만 사용되는 주소를 생성하는 데 사용, 2)transaction change를 돌려받는 데에 사용된다

 외부 체인은 1)해당 계정의 주소를 공유하는 데 사용, 2)금액을 받을 때(e.g. for receiving payments) 사용된다

  

bip44: [https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki)

등록된 코인타입 확인: [https://github.com/satoshilabs/slips/blob/master/slip-0044.md](https://github.com/satoshilabs/slips/blob/master/slip-0044.md)

  

  

#### BIP32, BIP 44 차이점

> BIP32에서는 마스터 키를 생성하고, 이를 사용하여 다수의 파생된 키를 생성할 수 있다. 마스터 키는 시드 문구(seed phrase)에서 생성되며, 이를 기반으로 파생된 키들을 생성할 수 있다. BIP32에서는 파생된 키를 사용하여 지갑 주소를 생성하는 방식을 사용한다.

  

> 반면, BIP44에서는 파생 경로를 사용하여 지갑 주소를 생성한다  
> BIP44는 BIP32의 확장된 버전으로, 다양한 코인 타입을 지원하는 지갑을 쉽게 구현할 수 있다

  

  

#### Mnemonic code로부터주소 생성해 보기

[https://iancoleman.io/bip39/](https://iancoleman.io/bip39/)

  

  

참고 :

[https://github.com/bitcoin/bips](https://github.com/bitcoin/bips)

[https://github.com/satoshilabs/slips](https://github.com/satoshilabs/slips)

[https://steemit.com/hive-101145/@happyberrysboy/happyberrysboy-posting-2023-02-27-16-13](https://steemit.com/hive-101145/@happyberrysboy/happyberrysboy-posting-2023-02-27-16-13)