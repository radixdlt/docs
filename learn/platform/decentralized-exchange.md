# Decentralized Exchange

## What is a decentralized exchange?

Technically, decentralized exchange is a market structure made up of a network of computers without relying on a  central authority like Nasdaq which hold the customers funds. Instead, trades occur directly between users \([peer to peer](https://en.wikipedia.org/wiki/Peer-to-peer)\) through an automated process without the need of a central authority. 

Because users do not need to transfer their assets to the exchange, decentralized exchanges reduce the risk of theft from [hacking of exchanges](https://en.wikipedia.org/wiki/Cryptocurrency_and_security). Decentralized exchanges can also prevent price manipulation or faked trading volume through [wash trading](https://en.wikipedia.org/wiki/Wash_trade), and are more anonymous than exchanges which implement [know your customer](https://en.wikipedia.org/wiki/Know_your_customer) requirements.

There are some signs that decentralized exchanges have been suffering from low trading volumes and [market liquidity](https://en.wikipedia.org/wiki/Market_liquidity). On the other hand, some decentralized exchanges have proven instrumental in enhancing liquidity. Central to achieving this is by working with [fiat](https://en.wikipedia.org/wiki/Fiat_money) pegged cryptocurrencies. 

In a decentralized system the technology provides investors with access to various bid/ask prices and makes it possible for them to deal directly with other investors/dealers. This system can be achieved by creating proxy assets or through a decentralized multi-signature escrow system, among other solutions that are currently being developed.

A **decentralized exchange** has some combination of decentralized properties. At the moment this most likely means some mix of 

* on-blockchain trade clearing, 
* ability for users to retain control of their funds, and 
* hosting an orderbook in some decentralized manner \(this is currently inefficient with current levels of blockchain scaling\). 

They are mostly frontend apps for now. They may run on a decentralized exchange protocol. In the future they may not be a frontend, but rather nodes in a p2p network which relay orders to others, and have only programmatic interfaces. Early examples of decentralized exchanges with frontends include EtherDelta and OasisDEX. Neither currently use an underlying decentralized exchange protocol. They are small at the moment— EtherDelta does about 2% of the largest centralized exchange’s volume per day.

For a decentralized exchange to be successful, it has to be developed on a high-throughput protocol like Radix which have fast settlement time and can scale to support millions of transactions per second.  


## How does it differ from centralized exchanges?

Decentralized exchange systems contrast with the current centralized models such that the users deposit their funds and the exchange issues digital IOUs that can be freely traded on the platform. When a user asks to withdraw his funds, these are converted back into the assets and sent to their owner.

The four core functions of any exchange are capital deposits, order books, order matching, and asset exchange. In order to create a fully decentralized exchange \(DEX\), each of these functions must be decentralized. In most exchanges, only the asset exchange is decentralized, as the assets are cryptocurrencies deployed on the blockchain that no central entity controls. However, the other three functions, and especially capital deposits, are usually centralized. Due to [KYC](https://www.google.com/url?q=https://fin.plaid.com/articles/kyc-basics&sa=D&ust=1533674332985000) \(know your customer\) and [AML](https://www.google.com/url?q=http://www.finra.org/industry/aml&sa=D&ust=1533674332985000) \(Anti-Money Laundering\) regulations, exchanges are often required to seek users’ identities for capital deposits, creating centralized record-collection and data-storage of personal information. Effectively, centralized exchanges give users permission to transact currencies, rather than creating a permissionless ecosystem.

On an architectural level, decentralization means that there is no centrally-controlled server\(s\), and the networks’ nodes are distributed.

## Why do we require a decentralized exchange?

There are a few obvious benefits to decentralized exchanges. First, they **allow you to remain in control of your funds**. So no risk of the exchange being [hacked](https://steemit.com/bitcoin/@michaelmatthews/list-of-bitcoin-hacks-2012-2016) or [going insolvent](https://magoo.github.io/Blockchain-Graveyard/). This can lead to higher liquidity, as users may be willing to leave orders open on the orderbook for longer when counterparty risk is gone. Second, they create global order books. Decentralized exchanges are borderless and can serve anyone from any country.Third, they are **low friction. No signup required, just trade**.

Finally, they avoid the double on boarding problem that of buying bitcoins to trade between two different pairs of decentralized assets. Decentralized exchanges allow atomic swaps and some even let you exchange assets between multiple assets. 

For example if you want to buy dogecoin, you will have to first buy bitcoins with a fiat currency and then buy dogecoins. Moreover, you cannot trader dogecoin for cheesecoin both of which are registered on the same platform/protocol like ERC-20 tokens.

## Radix Decentralized Exchange

The Radix decentralized exchange is an application that is served by each node in the public network.  
It is a part of the tempo ledger and the exchange mechanism is defined at the protocol level.

It features a full set of functions like capital deposits, order books, order matching engine, and asset exchange. Assets are exchanged atomically instead as opposed to aggregated exchange.

### **Fund Management**

Radix leaves control of funds completely in control of the users. That said, in order to trade with other Radix assets you do not need to be move them into a smart contract. Funds are traded atomically at the protocol level, completely on-chain. Confusing I know. The bottom line is that at any point in time a user can withdraw or deposit Radix based tokens and assets without any third party intervention.

### **Trading Logic**

In Radix new market orders are stored “on-chain” in a globally available order book. On chain means they are stored in a shard. Contrary to popular decentralized exchanges all orders are stored on-chain for due to the low cost and high settlement speed. The following mechanism is used:

A person can submit open buy or sell orders for a given Radix token — in exchange terminology this person is the _Maker_. Another trader can browse these orders and choose to execute on them — this is called the _Taker_.

Now, the special sauce that makes on-chain order books work comes right from the heart of the Radix protocol—  a system of constraints that enable trustless matching of orders between the maker and the taker, without relying on a third party. 

At a high-level, this is how it works:

1. Maker creates a new order: Radix token, the amount,  and whether it’s a buy or sell order.
2. Maker creates a cryptographic hash of that order
3. Maker then uses their private key to sign the order hash
4. Maker sends order to shard together with the signature
5. When Taker wants to trade against the order, the signature and order information is send to the order books trade function.
6. Validators serving the order-book shard verify that the signature originated from Maker
7. The validators makes sure order is not expired or filled.
8. Funds are transferred and fees are taken by updating the state transition.



