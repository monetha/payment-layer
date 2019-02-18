# MONETHA: Decentralized Reputation Framework

## Payment Layer

* [Abstract](#abstract)
* [Payments Layer Design](#payments-layer-design)
  * [Actors](#actors)
  * [Implementation](#implementation)
* [Token Usage](#token-usage)
  * [Paybacks](#paybacks)
  * [Tokenholder program](#tokenholder-program)
* [Forking the Payment Layer](#forking-the-payment-layer)

## Abstract

The Payments layer of Monetha's Reputation Framework is a decentralized transaction layer based on enforceable contracts between the buyer and the seller (or service provider) with no involvement of a third party. It can be used alongside Monetha's Reputation layer or separately from it.

Merchants (for our purposes the term includes the providers of both goods and services) are ​able​ ​to​ ​accept​ ​Ethereum​ ​based​ ​cryptocurrencies​.

### Key Principles

* Make​ ​the​ ​payment​ ​process​ ​simple​ ​and​ ​efficient;
* Make​ ​accepting​ ​payments​ ​for​ merchants ​faster and more transparent;
* Bring​ ​the​​ ​Ethereum-based​ ​​token​ ​economy​​ ​to​ ​the​ ​​mainstream.

### Payments Layer Design

The design of the Payments layer resembles the escrow mechanism you would normally encounter in vending machines. There the customer's money is held in an "escrow area" until the transaction between the customer and the merchant, which results in an item exchange, is successfully completed. If a problem occurs and the merchant can no longer process the transaction, the customer can withdraw his or her funds. If no problem occurs, the funds are released to the merchant.

Our development is driven by the following principles:

* Open-source project;
* Misuse is easily detectable;
* User and developer friendly;
* Improvements are driven by the community via proposals, bug bounty and other means.

### Actors

**Customer**

A person or entity that purchases goods or services. Depending on the use case this can be an application representing an entity or person. Customers:

* Can interact with Merchants either directly through the Ethereum network or via other communication interfaces (service provider 3rd party applications);
* Do not have any particular requirements and in the majority of cases can be a simple Ethereum address;
* Receive a payback for each purchase. It is equal to 0.2% from the purchase amount in Monetha vouchers and can be used for applying discount for other purchases using Monetha's Payments layer;
* Participate in transactions with Merchants through their payment processor contract;
* Initiate the purchase flow.

**Merchant**

A person or entity that provides goods or services and would like to receive payment in an Ethereum based crypto-currency. Merchants:

* Own a set of contracts representing their entity on the blockchain;
* Maintain their operations history on the blockchain;
* Trigger escrow to release funds in case of successful operations;
* Have control over refunding or canceling an order.

**Monetha**

Monetha's role is to provide paybacks to customers using the payment gateway. Monetha:

* Provides a payback to all Customers making purchases from Merchants using Monetha's deployed and maintained MonethaGateway contract;
* Allows the application of customer discounts without the Merchant losing funds.

### Implementation

The implementation is designed to be as decentralized as possible to maintain a level of trust between the parties.

![payment-layer-concept.jpg](diagrams/payment-layer-concept.jpg)

#### Merchant's Contracts

* MerchantWallet:
  * Accepts payments for orders;
  * Acts as a Merchant's profile;
  * *Source:* [MerchantWallet](https://github.com/monetha/payment-contracts/blob/master/contracts/MerchantWallet.sol).
* MerchantDealHistory:
  * Stores the hash of deal conditions between the Merchant and the Customer;
  * Shows a history of all transactions made by a particular Merchant;
  * Enables to see the evolution of reputation points for both parties.
  * **TBD** The Merchant's deal history is used as a FactProvider in [reputation-layer](https://github.com/monetha/reputation-layer);
  * *Source:* [MerchantDealsHistory](https://github.com/monetha/payment-contracts/blob/master/contracts/MerchantDealsHistory.sol).
* PaymentProcessor:
  * Implementation of an escrow mechanism where Customers stake their money and wait for the goods/service to be exchanged;
  * The Customer can cancel the order;
  * The Merchant can refund the order after it has been processed;
  * *Source:* [PaymentProcessor](https://github.com/monetha/payment-contracts/blob/master/contracts/PaymentProcessor.sol).

#### Monetha's Contracts

* MonethaGateway:
  * Ensures that Monetha receives a 1.5% fee to be able to cover paybacks and discounts;
  * Ensures that the Customer's applied discount gets transferred back to him or her;
  * Ensures that the Customer receives a payback of 0.2%;
  * *Source:* [MonethaGateway](https://github.com/monetha/payment-contracts/blob/master/contracts/MonethaGateway.sol).
* MonethaVoucher (work in progress):
  * Allows MonethaGateway and other trusted parties to exchange vouchers into ether to return the discount amount to customers;
  * Registers and tracks the amount of vouchers that are emitted as paybacks to Customers;
  * *Source:* [MonethaVoucher](https://github.com/monetha/payment-contracts/blob/master/contracts/MonethaVoucher.sol).

#### Implementation examples

* Monetha's payment gateway [https://www.monetha.io/e-commerce](https://www.monetha.io/e-commerce)
* ICO pass analyzer [website](https://icoanalyzer.monetha.io)
  * Github repository for web application [https://github.com/monetha/ico-analyzer-web-app](https://github.com/monetha/ico-analyzer-web-app)
  * Github repository for analysis function run as AWS lambda [https://github.com/monetha/ico-analyzer](https://github.com/monetha/ico-analyzer)

### Token Usage and Loyalty program

There are multiple directions how MTH tokens are being utilized in Monetha's ecosystem. Two of the main directions are Paybacks and Tokenholder programs in combination referenced as Monetha's Loyalty program. More token applications are to come while using our [Reputation layer](https://github.com/monetha/reputation-layer)

#### Paybacks

Paybacks are being granted to all customer's who have made an Ether payment through Monetha's payment gateway. A customer receives a payback in Monetha Vouchers (aka MTHV). Each MTHV is backed by a MTH token. Payback voucher amount is locked at the capacity of MTH tokens staked in the MonethaVoucher contract by Monetha.

Amount of vouchers granted to a user as a payback is converted according exchange rate that is being updated by Monetha on daily basis.

This is done with a help of a [MonethaVoucher](https://github.com/monetha/loyalty-contracts/blob/master/contracts/MonethaVoucher.sol) smart contract which is deployed to Ethereum mainnet and can be found at address [0x8D6b6F21e4519Ec10F54842CD9C113eE7e50E04a](https://etherscan.io/address/0x8D6b6F21e4519Ec10F54842CD9C113eE7e50E04a)

Monetha vouchers are valid for six months. If unused within that period, they will move back to their respective pools. Monetha vouchers are non transferable

#### Tokenholder program

All people that have bought or received MTH tokens (aka tokenholders) are eligible to participate in a monthly tokenholders program. It is the program where Monetha stakes 1/3 of monthly revenue that was acquired via Monetha payment gateway allows MTH tokenholder to ​claim​ ​for​ ​vouchers proportionately​ ​to​ ​the​ ​amount​ ​of​ ​MTH​ ​tokens​ ​that​ ​they​ ​hold.

This is done with a help of a [MonethaTokenHoldersProgram](https://github.com/monetha/loyalty-contracts/blob/master/contracts/MonethaTokenHoldersProgram.sol) smart contract which is deployed to Ethereum mainnet and can be found at address [0x4830e1e8D533313d2bb426cF1EF306460f288524](https://etherscan.io/address/0x4830e1e8D533313d2bb426cF1EF306460f288524)

#### How​ ​it​ ​works

* Let’s​ ​say​ ​we​ ​have​ ​1000​ ​investors​ ​as​ ​MTH​ ​holders,​ ​holding​ ​1​ ​MTH​ ​each.​ ​Now,​ ​all
token​ ​holders​ ​equally​ ​hold​ ​0.1%​ ​of​ ​the​ ​total​ ​token​ ​supply.
* Let’s​ ​say​ ​Monetha’s​ ​merchants​ ​sold​ ​100​ ​000​ ​ETH​ ​worth​ ​of​ ​goods​ ​and​ ​services​ ​in one​ ​month​ ​and​ ​let’s​ ​say​ ​that​ ​1ETH​ ​=​ ​1MTH.​ ​Because​ ​Monetha​ ​takes​ ​1.5% transaction​ ​fee​ ​from​ ​merchants,​ ​Monetha​ ​will​ ​have​ ​1500​ ​ETH​ ​of​ ​revenue​ ​collected.
* For​ ​this​ ​scenario,​ ​1⁄3​ ​of​ ​Monetha’s​ ​revenue​ ​means​ ​1⁄3​ ​of​ ​1500​ ​ETH​ ​=​ ​500​ ​ETH. Therefore,​ ​​ ​we​ ​will​ transfer ​500​ ​ETH​ to`MonethaTokenHoldersProgram` contract and initiate a purchase of MTHV tokens for that amount. As a result ​we get​ ​500​ ​MTH​V ​that​ ​is​ ​available​ ​to​ ​use​ ​for​ ​MTH​ ​holders​ ​in​ ​Monetha’s ecosystem.​ ​
* All​ ​1000​ ​MTH​ ​holders​ ​will​ ​equally​ ​have​ ​the​ ​ability​ ​to​ ​use​ ​500​ ​MTHV ​when​ ​shopping​ ​with​ ​Monetha’s​ ​merchants.​ ​In​ ​this​ ​case,​ ​one​ ​token​ ​holder will​ ​be​ ​able​ ​to​ ​spend​ ​up​ ​to​ ​0.1%​ ​of​ ​500​ ​MTH​ ​(=0.5​ ​MTH​ ​each).

#### Program lifecycle

1. Monetha transfers 1/3 of the revenue that were residing in temporary vault address [0x003a9f226b282539b161f8704b8dbc783c2f7860](https://etherscan.io/address/0x003a9f226b282539b161f8704b8dbc783c2f7860) to a [Monetha Tokenholder contract address](https://etherscan.io/address/0x4830e1e8D533313d2bb426cF1EF306460f288524)
1. In order to participate in the program tokenholders must transfer their MTH tokens on the 1st day of the month. To do so a person must: 
    1. Approve transfer of the tokens for the "Monetha Tokenholder program contract" from their address. This can be achieved by executing `approve()` on MTH token smart contract 
    1. Execute `participate()` method of the [Monetha Tokenholder contract](https://etherscan.io/address/0x4830e1e8D533313d2bb426cF1EF306460f288524)
1. After 1st day passes a tokenholder can execute `redeem()` method of the [Monetha Tokenholder contract](https://etherscan.io/address/0x4830e1e8D533313d2bb426cF1EF306460f288524) which will result in granting proportional share of MTHV for Monetha revenue share
1. Day before next month all of the vouchers will be converted into Ether and a new cycle will begin.

If revenue was not claimed it will be transferred to the next month and tokenholders will be able to claim it again.

Token holders can forfeit from the tokenholder program even if they already staked their tokens by executing `cancelParticipation()` method of the [Monetha Tokenholder contract](https://etherscan.io/address/0x4830e1e8D533313d2bb426cF1EF306460f288524)

### Forking the Payment Layer

Our implementation can be forked or similar ones can be built.

The solutions are in favor of our implementation:

* Provide a valuable and useful solution;
* Offer paybacks and discounts maintained and guaranteed by Monetha.
