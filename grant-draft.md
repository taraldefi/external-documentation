
https://taraldefi.github.io/

# Short Deck/Intro
https://app.pitch.com/app/presentation/319f13c5-661b-4c07-8e6a-f4f6cb8d6f86/87dc9b45-eabd-4b24-9520-c0e7e059466a

# Background 
## What problems do you aim to solve?

Ultimately, we are building a platform that enables "farmers" to generate stable yield from high-quality trade finance loans, protocol participants to gain upside in the insurance protocol, and buyers and sellers of international goods to securely release payment on delivery and utilise short duration loans for their inventory financing. For the scope of this grant, we will be focusing specifically on delivering smart contracts and a basic UI that demonstrates demonstrates the ability for buyers and sellers to confirm their purchase and capture the transaction data in an NFT that will act as collateral for lenders.

The international payments market between international suppliers and buyers of goods is fraught with risks of non-payment. Letter of Credit transactions are used to intermediate between parties and to provide a level of assurance to counterparties that delivery will take place before funds are released by buyers. Banks are currently the centralised parties of trust for these transactions, leveraging their reputation in ensuring security and trust. The global Letter of Credit market is $2.8 trillion per year and the uncovered international "open book" market is ca. $8 trillion per year.

Taral will solve this by allowing buyers and sellers to transact directly with one another -- at a lower cost, better user experience, and higher speeds.

# Project Overview
## What solution are you providing?
Letter of credit products are often costly and present issues when managing this in a smaller environment. Fundamentally, we find that these costs are prohibitive and can cost as much as 5-8% for a letter of credit for smaller transactions.

This can be resolved with smart contracts. By ensuring that funds are only released when the party has shipped the goods or when the buyer has confirmed that the goods have been received, we can demonstrate a way of maintaining custody of the funds bringing security to both parties. 

Two additional pools of liquidity will be created. One will be a pool that will cater to providing insurance against defaults and the other a liquidity pool for providing cashflow and loans to the involved parties.

Investors will have the opportunity to lock up funds and earn a set return based on the return generated from the insurance premium charged for the individual transaction between buyer and seller.

The liquidity pool will allow investors to either select the individual loans that they want to finance (30/60/90 day duration). These will have a varying yield based on the collateral supplied and the individual risk in the transaction.

## Why is this project beneficial for the Stacks community? How does it serve the mission of a user-owned internet?

By collateralising real-word financial assets (supply chain finance) on chain, we help to move the world further towards a user owned internet and drive real-world, daily, business transactions on DeFi. Given the team's background in the cross-border trade finance space, this marks the beginning of bringing a diverse range of high-quality, real-world assets to Stacks.

These assets are traditionally stable-yield assets reflecting the ethos of the Stacks community that has grown around a stable, proven, and highly-secure protocol.

After the recent launch of Arkadiko bringing with it the ability to mint USDA, Taral brings a real-world application for stablecoins. By allowing trade finance loans to be bridged to the Stacks ecosystem,  this project will be beneficial for DeFi users, opening up a channel to real-world assets and diversifying the range of assets available.

Reputable asset originators are able to source credit lines for high-quality financial assets, starting with import/export working capital financing.

As Taral will not be acting as a custodian of the funds, Taral will create a DAO that will allow for protocol participants to contribute to the governance of the protocol.

### Why Stacks?

Stacks is built on Bitcoin, one of the most established and well known cryptocurrency protocols. By building on Stacks and therefore on Bitcoin, we are building on a secure, well established protocol, with a small attack surface. This does not necessarily guarantee the security of the protocol, but it does lend a great deal of credible, proven support from the start. Other systems are yet to develop the track record and some are still open to attacks. Without opining on the quality of the code and the method of attack used, the unfortunate attack on the Poly Network is an example of where vulnerabilities can be exploited by hackers to steal funds huge amounts of funds. The size and potential volume of the market means security is paramount for institutional money to follow.

# Team

## Doru:
* Worked on a multitude of products in fintech and ecommerce
* Lead and built teams of successful developers
* Served as architect on various projects
* Designed and coordinated teams throughout the whole product lifecycle

## Vincent:
* Conducted user interviews with Euler Hermes, partner bank to understand their processes and integrations requirements, translating into detailed business requirements
* Conducted user interviews with exporters and importers to understand and prioritise key product features including Indicative Offer Analyse, Contract Management and E-Signature.
* Wrote underwriting documentation requirements, created initial mock-ups, gathered metrics and set up analytics tools before and after product launch
* Led OKR setting on an organisational level and also at a product-team level; translating the product roadmap into team OKRs
* Led two-week agile sprint planning alongside CTO (scrum master) cross-functional teams of developers, designers and QA both onsite and remote development teams in other time zones by utilising project management 2.0 tools such as JIRA & Slack

## Additional Community Developer:
* We're building something ambitious that will require a lot of help! Please reach out if we have piqued your interest

# Risks
## What dependencies or obstacles do you anticipate?

* I am the sole developer on Taral right now and for professional reasons can't go full time on this. The plan is to dedicate 10-12 hours each week building Taral. Time and time again I've been involved in projects with a large technical undertaking so I'm no stranger to the technical difficulties an innovative project like Taral might bring to the table.
* As the Stacks ecosystem is still young, there are some pieces missing such as chainlink integration which we'd need for the price feeds of on-chain and off-chain assets. The plan is to start with an own oracle implementation that would ensure on-chain assets price is up-to-date
* On-ramp and off-ramp is currently hard to integrate but with USDC planning to expand to stacks — then it will be possible.
* Swapping bitcoin with TAL won't have an excellent user experience since it doesn't cover bitcoin transactions (yet).
* Commercial risk: Unfriendly/confusing UI. Given that a large proportion of customers will still require fiat tokens, there will also need to be an on- and off-ramp piece that allows participants to access their funds and similarly repay loan amounts to the protocol.
* Fraud risk: Bad actors
* Technical risk: Security. Security is of most importance in DEFI. Happily Clarity is a decidable language and has extensive mechanisms to ensure security. The smart contracts will be secured, unit tested and battle tested to ensure security.

## What contingency plans do you have in place?

* Commercial risk: Test rigorously with customers before investing excessive resources in development. Product demand is there, as demonstrated by the $2.8 trillion Letter of Credit market and the $8 trillion open trade that occurs every year
* Distribution, will be the issue. However, with a proposition that is sufficiently faster, more accessible, and cheaper we will see a highly competitive proposition.
* Fraud risk:Ensure that we work with trusted companies within our network initially
* Technical risk: Ensure security of the product and have tests for pessimistic scenarios.

All estimations below are in hours and are primarily covering the time for smart contracts and web application development. A small hourly allocation per user story has been included for design (1-5 hours depending on complexity of the user story). The Taral team is, of course, happy to be paid in STX.

We are keen to hear from other community members with capacity and an interest in helping bring real-world assets to Stacks.

### Milestone 1: $13,350

Final deliverable:

| User Story ID | Description | Estimation |
| ----------- | ----------- | ----------- |
| 1 |	I need to be able to submit the details of my purchase order | 140 |
| 2 |   I need to be able to view and sign or disapprove the purchase contract | 19 |
| 7 |	I need to be able to input and approve purchase terms agreed with importer | 29 |
| 19 |	I need to have a web boilerplate | 34 |
| 14 |	I need to be able to see and review the underlying asset into which I am providing liquidity | 45 |

### Milestone 2: $6,250

Final deliverable:

| User Story ID | Description | Estimation |
| ----------- | ----------- | ----------- |
| 3 |   I need to be able to select the payment terms (when payment should be released & over which duration I would like to repay the outstanding amount) | 13 |
| 5 |	I need to be able to confirm receipt of shipment of goods and thereby release remaining agreed funds to exporter | 53 |
| 9 |	I need to be able to upload proof of delivery/shipment of goods | 59 |