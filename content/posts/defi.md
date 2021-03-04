---
title: "Finematics (DeFi)"
date: 2021-01-31T20:34:27-05:00
draft: true
categories: ["crypto"]
series: ["DeFi"]
ShowToc: true
---
Sources: [Finematics](https://finematics.com/guide-to-decentralized-finance/)

**Note: I'm already familiar with the basics of DeFi (eg. what it is, Uniswap, MetaMask) so there won't be any notes about those topics. Other topics won't be as comprehensive. Refer to the Finematics link if you want to start from the beginning, it's an excellent resource**

View my project specific posts:

---
## Lending and Borrowing 
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
- **Deterministic Algorithm/Automated Market Maker:** mechanism that adjusts price after every trade according to a deterministic algorithm 

Price is determined by ratio of the tokens. Price impact (slippage) determined by two factors: 
- size of the trade
- how much liquidity is in the pool (why some protocols like BAL started incentivizing LPs by giving them extra tokens. This is called **liquidity mining**)

Uniswap: Constant Product Market Maker
`calculation: x * y = k`
- product of quantities of two tokens (x and y) remains the same (k) 
- in this method, Uniswap can provide liquidity no matter how large the trade because the algorithm asymptotically increases the price as quantity demanded increases 

Balancer: as many as 8 tokens in a pool

Curve: realized AMM doesn't work well for assets that should have similar prices (eg. stablecoins and different versions of synthetics like SETH and WETH) 
- offers lower fees and lower slippage when exchanging these tokens

---
## Impermanent Loss
Temporary loss of funds when providing liquidity. Difference between holding asset and providing liquidity  

Observed when LP has to provide both sides of the pool in correct ratio and rely on arbitrage to ensure the prices reflect real world prices 

**Refer** to example in video, very clear

What are the incentives for LPs? 
- Fees (0.3% on every trade for Uniswap). In order to be profitable, impermanent loss < collected fees
- **liquidity mining:** rewarding LPs with extra tokens  

Pools other than Uniswap:
- Curve: pools have very little impermanent loss because they only hold tokens that should be very similar or the same in value with the other. In general, stable pools will attract more liquidity
- Balancer: arbitrary weights in pool. LPs can have high exposure to a certain asset by providing much higher weights. Reduces impermanent loss
- Bancor V2: adjust weights automatically based on external prices feeds coming from price oracles  

Things to research: 
- Alpha Homora
  - https://defipulse.com/blog/alpha-homora-v2
- BadgerDAO
