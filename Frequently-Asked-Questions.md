### What exactly does Wanchain do?
Wanchain is implementing cross-chain transfers of assets by setting up connections between the accounts of different blockchains and providing a framework for financial applications based on digital currency and digital assets. Wanchain is not just a cross-chain platform, performing cross-chain transactions and interconnections between multiple assets, it is also a blockchain network that can operate independently. It includes native coins, supports intelligent contracts and owns the privacy protection mechanism for transactions.

### What do the key traits distinguish you from your competitors?
Some consider that the cross-chain related projects are all our competitors. In fact, we are more cooperative rather than competitive (See https://medium.com/helloiconworld/blockchain-interoperability-alliance-icon-x-aion-x-wanchain-8aeaafb3ebdd for further details about the Interoperability Alliance).

Wanchain would like to rebuild a financial infrastructure with three key characteristics: cross-chain + privacy protection & smart contract, which means that the assets from the other chains can be moved to Wanchain and be created cross-chain smart contract with privacy protection. This is unique. Frankly, there isn’t any other existing project that can cover the total three characteristics. Wanchain 1.0 is live with the explorer, wallet, native coins, smart contract functionality and privacy protection. Wanchain 2.0 is coming out this summer with PoS and cross-chain functionality with Ethereum.

### How is Wanchain's cross-chain trading different from atomic swaps?
For the atomic swaps, you must implement and modify the original chain. The parties involved in this atomic swap must go through many procedures. It is very difficult to build a decentralized exchange or any advanced applications on top of atomic swaps, so the user experience is not so good. For Wanchain, users can move assets to this platform and have those “proxy tokens” transferred within the platform much faster than the current state of atomic swaps (HTLC protocol). Once the applications are on the Wanchain, we can create complicated cross-chain transactions with the use of Wanchain smart-contracts. Atomic swaps cannot do that.

### How is Wanchain treating scalability?
Scalability and performance are the big challenge for the whole industry. Communities, including Ethereum community are looking at many solutions. We will keep close with those projects in the communities whilst also having a dedicated team researching about the best practices.

We are also partnering with several of the top research institutions in the world to have them research into scalability and offer commits to our code. These partnerships will be revealed in the coming months. Stay tuned!

### How do you plan to scale to meet the transaction requirements of the global financial system?
The global financial system is a huge and complicated system. There isn’t any existing system that can cover every aspect.

Our development team is in continuous progress. Along with the development of blockchain technology, traditional financial systems will gradually embrace this new technology. So, what we think the most important thing at this moment is not the throughput itself but the use of blockchain on real application scenarios.

### Wanchain will feature masternodes: cross-chain transaction proof nodes (vouchers), locked account management nodes (storemen), and general verification nodes (validators). Could you please elaborate a bit on their roles?

The verification nodes on Wanchain are divided into three categories: Cross-chain transaction proof nodes (Vouchers), locked account management nodes (Storemen) and general verification nodes (Validators).

Vouchers are used to provide proof of transactions between the original account and the locked account. A Voucher is required to pay a certain security deposit. The higher the deposit, the greater chance the proof if provides will be adopted. If the proof is found to be false, the security deposit will be deducted from the holding account and the authorization of the Voucher is revoked.

The Storeman, upon receiving a notice, is responsible for computing the signature shares according to its own part of the key and merging the signatures parts into a complete signature for the lock account. When this is done, operations related to the locked account are performed.

The Validator informs the Storeman of operational actions related to the locked account and complete the record of operations on the Wanchain whenever a transaction proof reaches a consensus.

Please note, we cannot comment on the amount of coins needed for a node. This will first be announced around the release of Wanchain 2.0.

### How is Wanchain's marketing expansion and community development?

Wanchain really focuses on this. We reserved some tokens for marketing, business development. We use these tokens for our bounty system for people around the world to support, give presentations and expand the overall Wanchain presence globally.

We have a vibrant group of community managers from across the world. Our main offices are in Beijing and Austin, but also have a big presence outside of these regions.

All our community managers are really the key points of interface to the community. So, if anyone who is interested in and really wants to participate in this, feel free to contact us, and we would love to help and support you growing the ecosystem of Wanchain and share our vision of the world.

### What kinds of partnerships will Wanchain establish?

We have the interoperability alliance which we announced late last year as well as the KyberNetwork and recent Austin Blockchain Collective.

During the mainnet launch, we went through that how Wanchain plans to rebuild the financial industry and what technological and business partnerships are going to be built coming months. More details will follow in the coming months about the Wanchain Ecosystem and the Wanchain strategy moving forward.

### Address format rules
Wanchain account addresses are 20 bytes long. In the human readable form they are ASCII-encoded hex strings, such as: 0x731Bd7289b4191706b00f6f1877662B5E8697E82. The "0x" prefix is optional.
Even though they appear to look like Ethereum addresses - the Wanchain addresses are intentionally designed to be incompatible with that unrelated blockchain. This is to prevent user funds from being sent to an addresses created on the wrong blockchain (tools).

The difference comes from the capitalization rules of the letters in the addresses.
In case of Ethereum - capitalization of alphabet letters within the address is optional, and it's up to individual service providers to use it or require it.
In Wanchain's case all our tools insists on addresses being capitalized correctly, following the Wanchain address format rules.

For technically inclined, this rule is as follows: convert the address to hex, but if the ith digit is a letter (ie. it's one of abcdef) print it in lowercase if the ith bit of the hash of the address (in binary form) is 1 otherwise print it in uppercase.
Which happens to be the exact opposite of the Ethereum's EIP 55 suggested format; but again there is not guarantee that Ethereum tools enforce proper address checksumming as it was added to that ecosystem as an afterthought.

If a need arises we will provide an online conversion/checker tool for Wanchain addresses.
