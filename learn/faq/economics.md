# Economics

## FAQ

This document is a compilation of questions that have been asked by the Radix community members across different channels.

## Radix Token

### Is Radix token a stable coin?

Not exactly. The Radix native utility token, the Rad, is a low volatility token where its price is stabilised against the Radix Index Token. The Radix Index Token is backed by a flexible, weighted basket of tokens, which provides a diversified index for the Economic Algorithm to use for value stability targeting.

If the price of the Rad to Radix Index Token exceeds the set Price Ceiling the Economic Algorithm will automatically trigger the minting of new Rads. The Economic Algorithm then sells these Rads for Index Tokens on the Decentralised Exchange until the price returns within the target boundaries. Any Index Tokens the platform receives from selling Rads will be held in Reserves and may be used by the Economic Algorithm to buy Rads in the future. Any Rads received by the platform will be burned.

If the price for Rads in Index Tokens falls to or below the Price Floor then the Economic Algorithm will automatically buy Rads at the Price Floor using any Index Tokens it has in its Reserves. The system will then burn all Rads it buys, reducing the total supply and providing an effective floor for the Rad's price in Index Tokens. The minimum price the Economic Algorithm will pay increases with the growth of the Reserves.

As the system begins with no Reserves, it is expected that the price of the Rad will initially fluctuate significantly against the Index Token. The Economic Algorithm is designed to tend the system towards stability as the Reserves grow, building the Reserves and slowly bringing the Price Floor up towards the Price Ceiling, reducing the scope for any price volatility.

### **Is the Rad pegged to the USD?**

No. The price of Rads is stabilized against the Radix Index Token \(XRI\), not the USD. The Radix Index Token is designed to be a flexible, low volatile diversified basket of both suppliers \(Approved Minters\) and asset types.

In Phase 1, when the XRI is not live, Rads will be priced at 1 USD- backed token to 1 Rad token.

In Phase 2, when the XRI and Economic Algorithm are live, the system will price Rads at 1 Rad to 1 XRI token. It is expected that, initially, the majority proportion of the XRI will be composed of USD- backed tokens, which over time will be diversified to include more assets such as other currencies or commodities.

### **What is the difference between Rads, XRD and XRI?**

The Rad is the native token of the Radix platform. Rads and XRD are the same token; XRD is simply the ticker symbol \(exchange listing name\) for the Rad.  
The XRI is the Radix Index Token, which is a token backed by a weighted basket of assets.

### How do I buy Rads?

After Technical Go-Live \(Phase 1 - Bootstrapping\), but before Economic Go-Live, to purchase Rads you must first purchase dollar-backed tokens from an Approved Minter. Once you have dollar-backed tokens you send those tokens to the Reserve. The Rad Minting Algorithm then mints new Rads and sends them directly to the user’s wallet at a fixed exchange rate of 1 USD to 1 Rad.

After Economic Go Live \(Phase 2 & 3\) there are two ways you can buy Rads; either directly from the system or from another Radix user who already owns Rads.

If buying from the system you must first purchase the component tokens, which are included in the XRI \(Radix Index Token\), from Approved Minters. You must then send these tokens in the correct proportion to the XRI minting contract which will then mint XRI tokens. You can then use these XRI tokens to buy Rads directly from the system on the Decentral Exchange \(DEX\).

If buying Rads from another user you simply place a buy order on the DEX for the amount of Rads you want to buy and at what price.

[How to buy Rads Infographic](https://uploads-ssl.webflow.com/5a5a128611f84b0001e85b9f/5c5315b1d8fe7db52ca1e65b_How%20to%20buy%20or%20sell%20Rads.png)

### **When can I buy Rads?**

You will be able to buy Rads at Technical Go-Live, which is [currently scheduled](https://www.radixdlt.com/roadmap) for Q4 2019.

### **With which currencies can we purchase XRD with?**

In Stage 1, after Technical Go Live and before Economic Go Live, you will be able purchase Rads from the system using USD- backed tokens from a group of Approved Minters. In Stage 2, after Economic Go Live, you will need Radix Index Tokens to buy Rads from the system.

You will also be able to buy Rads from other users using their preferred currencies. Please see [this infographic for a simple guide on how to buy Rads](https://uploads-ssl.webflow.com/5a5a128611f84b0001e85b9f/5c5315b1d8fe7db52ca1e65b_How%20to%20buy%20or%20sell%20Rads.png).

### **Is the Rad a security?**

The Rad is fundamentally required to use the system, to pay the fees for transactions across the network. It is how the Node Runners that maintain the network are paid and incentivised.

Whether or not the token is viewed as a security depends on the jurisdiction and the state / regulatory view within that jurisdiction.

### **Will Radix have KYC?**

The Radix decentralized exchange will not have KYC. But fiat on/off ramps should follow standard KYC and ****AML checks.

## Radix Index Token \(XRI\)

### **How can I buy XRI in Stage 2 after Economic Go-live?**

If buying from the system you must first purchase the component tokens from Approved Minters which are included in the XRI \(Radix Index Token\). You must then send these tokens in the correct proportion to XRI minting contract which will then mint XRI tokens. These tokens will the be transferred to your wallet.

If buying XRI from another user you simply place a buy order on the DEX for the amount of XRI you want to buy and at what price.  
[How to buy Rads infographic](https://uploads-ssl.webflow.com/5a5a128611f84b0001e85b9f/5c5315b1d8fe7db52ca1e65b_How%20to%20buy%20or%20sell%20Rads.png).

### **What type of assets are to be included in the XRI?**

For the Index Token to be a good stability reference each asset in the basket should be selected for both liquidity and price stability. Due to the highly liquid and price resistant nature of fiat, fiat backed tokens present the best initial asset class for this role.

In the future other asset types, such as commodity backed tokens could be considered for inclusion in the XRI as long as their liquidity levels were considered to be sufficient.The choice of assets in the XRI basket is a question of governance.

### **Will Bitcoin ever be part of the XRI?**

BTC is currently, and likely to remain, a highly volatile asset. Including BTC in the XRI might significantly impact the stable value of the XRI the Radix Economic Model is looking to achieve. As the XRI is the only measure the Economic Algorithm uses to understand the price of the Rad, this volatility in the price of the XRI would lead to volatility in the price of the Rad. This goes against the ultimate goal of the Radix Economic Model; to create a token with strong price stability and therefore BTC will not be included in the XRI for the foreseeable future.

### **Is XRI a security?**

The XRI is a basket or container that you can put tokens into and remove tokens from. You can only get out the tokens that were put in and therefore there is no expectation of value increase or return on investment from the XRI token.

Whether or not the token is viewed as a security depends on the jurisdiction and the state / regulatory view within that jurisdiction.

## Genesis Atom

### **How is the genesis amount of Rads defined?**

Total genesis supply: 78,926,251.20 Rads

62.58% - Genesis Community

15.20% - Radix Foundation

12.42% - Radix Development Rebate

9.80% - Community Fund

Radix \(formerly eMunie\) has been in development since 2012. Those that were part of the eMunie community up until 2015 were given an early opportunity to contribute to the project. In total 127 community members donated the equivalent of 2,496.82 Bitcoin since 2013.

Due to the length of time between donation and the delivery of the project, we have converted the BTC, LTC etc received into Rads at the same price date of 17th December 2017, which was the Bitcoin USD high water mark: $19,783.06. This translates to 49,394,688.40 Rads.

Previously we had said that we would convert the early contributions at whatever price BTC was when we went live, at a $1 to 1 Rad ratio. If we had launched \(which we had ambitions to do\) in Dec 2017, then that would have been the value it was converted at.

Since then, we decided that the uncertainty of not knowing what the genesis supply would be due to market fluctuations, was a bigger risk to the system than just agreeing on a number and locking that in.

The holding size of contributors is pretty evenly distributed, with the largest single holder of genesis tokens represents 8.14% of total contributions, with median holding being 0.21%

## Approved Minters

### Will the Radix Team/Radix Foundation create an asset backed token or is it completely external?

The Radix Team/Foundation will not create any asset backed tokens. All Approved Minters will be separate entities from the Radix Foundation or renamed Radix DLT Ltd entity. A key reason for this is to reduce centralisation within the system.

### **How many Approved Minters does the team estimate there will be?** 

Our hope is to have between 3-5 Approved Minters by Technical Go- Live; to aid easy onboarding for users wishing to buy Rads and also to reduce any centralization risk by just relying on one1 Approved Minter.

For more information on the management of Approved Minters please see the [Approved Minters](https://papers.radixdlt.com/economics/latest/economics.pdf) section of the Economic Model paper.

### **What if there are no Approved Minters ready at Technical Go Live?**

Approved Minters are an essential element for the Radix Economic Model to function. It is therefore a key objective for the team to ensure that there will be several Approved Minters onboarded and ready for Technical Go Live.

If, near the planned Technical GoLive date, there are still no Approved Minters confirmed the Technical Go-Live may need to be delayed.

### **Can an entity become an Approved Minter after Economic Go live?**

Yes, new Approved Minters and component tokens of the XRI can be added through a governance decision. For more information please see the [adding new Approved Minter Tokens](https://papers.radixdlt.com/economics/latest/economics.pdf) section of the Economics Model paper.

### **What happens if a AM gets hacked and they a\) create infinite Rads and; b\) remain undetected, distributing small amounts from time to time to themselves?**

It is expected that Approved Minters will be required to undergo and complete regular audits which will be made public. As a result, Approved Minters would be unable to hide the discrepancy for a long period of time. Once the discrepancy was identified the Approved Minter’s token would be removed from the XRI, as explained in the [Index Token Administration](https://papers.radixdlt.com/economics/latest/economics.pdf) section of the white paper.

We are also exploring what technical solutions could be put in place to help mitigate this risk.

## Price Stabilisation **& Reserve**

### **Is there any upper limit to the Reserve or will the algorithm try to reach 0.9 Price Floor no matter how big the reserve might be?**

There is no upper limit for the Reserve. The Reserve will continue to grow until it can reach and maintain a Price Floor of 0.9 for every Rad in existence.

### **Where and how is the Reserve held?**

Unlike the traditional concept of a Reserve it is not held or controlled by any individual, or organisation, but is controlled by the Economic Algorithm itself. The ruleset for the Economic Algorithm is run by each node as part of the Radix protocol.

When an event on the ledger triggers the need for use of the Reserves it is subject to the network consensus process, validated against the Economic Algorithm ruleset. This process ensures the Reserves can operate autonomously, auditably and securely.

### **Will the current Price Floor be publicly visible?**

Yes, you will to see it on the DEX in the form of a large buy or sell order from the Economic Algorithm on the DEX.

### **What happens to the Price Floor \(Pmin\) in a crash? If a huge number of people dump their Rads, will that break Pmin?**

No - the Price Floor is set at the level that the Economic Algorithm can repurchase every single Rad in circulation without having to change the Price Floor. As it buys back Rads it burns them, reducing the total number of Rads as the Reserves are also depleted. This means that even in a massive crash scenario, the Price Floor stays at the same level throughout.

### Redistribution

### **Does the claim of Redistributed Rads happen automatically?**

No, you have to make a claim. Otherwise the system would have to review all balance holders which would be a large demand on resources.

Instead the expectation is that your wallet will make the claim automatically for you when it is open/running. The network will then check and return the correct balance, including any redistributed Rads.

### **What happens if a balance holder does not claim their redistributed Rads?**

Nothing. No one, including the System or the Foundation can claim the tokens. In effect this leads to fewer Rads being in circulation, therefore increasing the value of the Rads in circulation.

It should be noted that unclaimed redistributed Rads are included by the Economic Algorithm in its calculation of the total circulation of Rads and the current Price Floor.

### **What happens if I spend during an economic period?**

The exact mechanics are still being worked out but we are expecting that you will be able to make a proportional claim depending on what percentage of the period the tokens were in your account. This will depend on the mechanics of the implementation.  


## Incentives

### What is the incentive to setup/run a full-node?

Simply, the more services - which includes number of shards - that a node provides for the Radix network, the larger the potential earning base. Each node earns fees from any Temporal Proof's they create. The distribution of which nodes provide the temporal proofs is probabilistic, not resource intensive. In other words, you don't have to ‘mine a block’ to get TX fees, just confirm a transaction!

A machine will automatically maintain as many shards as are possible, given its resources; very small machines will run only a few shards, large machines many more. 

For more information, see: [https://papers.radixdlt.com/incentives/](https://papers.radixdlt.com/incentives/)

