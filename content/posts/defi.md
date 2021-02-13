---
title: "DeFi"
date: 2021-01-31T20:34:27-05:00
draft: true
categories: ["crypto"]
series: ["DeFi"]
---
Sources: [Finematics](https://finematics.com/guide-to-decentralized-finance/)

**Note: I'm already familiar with the basics of DeFi (eg. what it is, key projects, Uniswap, MetaMask) so there won't be any notes after those things. If you wish to start at the beginning, the finematics article is a great start**

View my project specific posts:

---
## Lending and Borrowing: 
One of the most important aspects of any financial system

**money market**: area of traditional finance that specializes in short term lending and borrowing

In crypto space, lending and borrowing occurs in two ways: 
- DeFi: AAVE, Compound etc
- CeFi: BlockFi, Celsius etc.

Value Proposition of DeFi Lending: Borrowing and lending in a decentralized, permissionless way while retaining full custody of your coins

Currently, all defi loans are over-collateralized (possible innovation opportunity?)

**over-collateralized**: borrowers has to supply collateral that is worth more than the loan they want to take. Why would somebody do this?  
- don't want to sell their tokens 
- delay or avoid capital gain taxes
- increase leverage

**collateral factor**: determines how much can be borrowed based on the quality of the collateral (eg. DAI and ETH is 0.75)  
- when borrowing, borrowed amount must stay below their collateral times collateral factor
- if it goes lower, they will have their loan liquidated

interest APY - calculated per Ethereum block (meaning variable APY)  
*Sidenote:* Seems like an obvious limitation of def; a variable APY that's ever-changing and volatile requires people to constantly check in on their APY to ensure they don't get liquidated (AAVE's stable APY is average of last 30 days which is still relatively short). For long investments, like mortgage, there has to be a way to have a long-term stable APY 

Example: 
- User deposits 10ETH in Compound, gets a certain number of CETH in return based on exchange rate (eg. new market set to 0.02)
- CETH then accumulates interest with each ETH block (exchange rate increases depending on supply APY)
- AAVE is similar except ATokens are pegged to underlying tokens at 1:1 

---
## Liquidity Pools
Traditional stock and crypto exchanges (eg. Coinbase and Binance) use an order-book model:
- bidders try to buy for as low as possible while seller try to sell for as high as possible. They then converge on a price
- **Market makers:** entities who are willing to sell at a particular price (buy order, sell order). Without market makers, exchange instantly becomes illiquid

Order-book models in DeFi: 
- this type of exchange is not replicated in DeFi because it would be slow, expensive and have poor UI/UX
- Ethereum has throughput of 12-15 transactions per second and 10-19s block times. Not viable for the mass quantities of every-changing orders on traditional exchanges. It also has a gas fee for every interaction with a smart contract so market makers would go bankrupt
- second layer solutions also don't work because of liquidity issues and transactions required to be sent in & out of 2nd layer

**Liquidity Pools:** Pools of tokens that are locked in smart contracts
- each pool holds two tokens, creating a new market for that pair (eg. DAI/ETH)
- first LP for a pool sets the initial price for the assets. 
- there is an incentive for first LP and every LP afterwards to provide equal value of both tokens to prevent an arbitrage opportunity
- LPs get LP tokens back in proportion to how much they provided to the pool 
- each trade has a 0.3% fee which gets proportionally distributed to all LP token holders
- **Automated Market Maker:** mechanism that adjusts price after every trade according to a deterministic algorithm
Things to research: 
- Alpha Homora
  - https://defipulse.com/blog/alpha-homora-v2
- BadgerDAO
