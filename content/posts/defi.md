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

Things to research: 
- Alpha Homora
  - https://defipulse.com/blog/alpha-homora-v2
- BadgerDAO
