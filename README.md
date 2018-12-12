# MONETHA: Decentralized Reputation Framework

## Payments Layer

* [Abstract](#abstract)
* [Payments Layer Design](#payments-layer-design)
  * [Actors](#actors)
  * [Implementation](#implementation)
* [Token Usage](#token-usage)

### Abstract

Decentralized transaction layer which is based on enforceable contracts without third party between buyer and seller (or service provider). This layer can be used with or without our Reputation layer.

Merchants or Service providers are ​able​ ​to​ ​accept​ ​Ethereum​ ​based​ ​cryptocurrencies​.

#### Key principles

* Make​ ​the​ ​payment​ ​process​ ​simple​ ​and​ ​efficient;
* Make​ ​accepting​ ​payments​ ​for​ Merchants or Service Providers ​cheaper​, ​faster and more transparent;
* Bring​ ​the​​ ​Ethereum-based​ ​​token​ ​economy​​ ​to​ ​the​ ​​mainstream.

### Payments layer design

Payment layer design resembles an escrow that you would normally encounter in vending machines. Where the customer's money is held in an "escrow area" pending successful completion of the transaction of the items between customer and merchant. If a problem occurs and merchant can no longer process with a transaction a customer can withdraw his funds. If no problem occurs, the funds are being released to a merchant

Our development is driven by the following principles:

* Open-source project;
* Misuse is easily detectable;
* User and developer friendly;
* Improvements are driven by the community via proposals, bug bounty and other means.

#### Actors

**Customer**

Person or entity that is trying to purchase goods or service. Depending on a use case this can be an application representing an entity or a person.

* can interact with a merchant or service provider either directly through Ethereum network or via other communication interfaces (service provider 3rd party application)
* Customer doesn't have any particular requirements and in majority of cases can be a simple Ethereum address.
* Customer receives a payback for each purchase equal to 0.2% from his purchase in Monetha vouchers that can be used for discount with other purchases from merchants using Monetha payment layer
* participates in a transaction with a Merchant through his payment processor contract
* initiates the purchase flow

**Merchant**

Person or entity that provides goods or service and would like to receive payment in Ethereum based crypto-currency.

* owns a set of contracts representing her entity on a blockchain
* maintains operations history on a blockchain
* triggers escrow to release funds in case of successful operations
* has control of refunding and canceling an order

**Monetha**

Monetha entity provides paybacks for customers using the payment gateway.

* provides a payback to all customers making purchases with Merchants using Monetha's deployed and maintained MonethaGateway contract
* allows application of customer discount without Merchant's loss of funds

#### Implementation

Implementation is designed to be as decentralized as possible to maintain level of trust between 2 parties.

Example implementation:

* Monetha payment gateway [https://www.monetha.io/e-commerce](https://www.monetha.io/e-commerce)
* ICO pass analyzer **TBD: provide a link to repository and demo app url**

![](diagrams/payment-layer-concept.jpg)

##### Merchant's contracts

* MerchantWallet
  * Accepts payments for orders
  * Acts as a merchant's profile
  * *Source:* [MerchantWallet](https://gitlab.com/monetha/trust-reputation-smart-contracts/blob/master/contracts/MerchantWallet.sol)
* MerchantDealHistory
  * Stores hash of deal conditions between merchant and customer
  * Shows a history of all transactions made by particular merchant
  * History enables to see evolution of reputation points for both parties.
  * **TBD** merchant deal history is used as a FactProvider in [reputation-leayer](https://gitlab.com/monetha/reputation-layer)
  * *Source:* [MerchantDealsHistory](https://gitlab.com/monetha/trust-reputation-smart-contracts/blob/master/contracts/MerchantDealsHistory.sol)
* PaymentProcessor
  * Implementation of an escrow where customer stakes his money and awaiting for goods/service to be exchanges
  * Customer can cancel the order
  * Merchant can refund order after it has been processed
  * *Source:* [PaymentProcessor](https://gitlab.com/monetha/trust-reputation-smart-contracts/blob/master/contracts/PaymentProcessor.sol)

##### Monetha's contracts

* MonethaGateway
  * Ensures that Monetha receives 1.5% fee to be able to cover paybacks and discounts
  * Ensures that customer's applied discount gets transferred back to him
  * Ensures that customer receives a payback of 0.2%
  * *Source:* [MonethaGateway](https://gitlab.com/monetha/trust-reputation-smart-contracts/blob/master/contracts/MonethaGateway.sol)
* MonethaVoucher
  * Allow MonethaGateway and other trusted parties to exchange vouchers into Ether to return discount amount to customers
  * Register and track amount of vouchers that are emitted as paybacks to customers
  * *Source:* [MonethaVoucher](https://gitlab.com/monetha/trust-reputation-smart-contracts/blob/master/contracts/MonethaVoucher.sol)

### Token Usage

**_TBD_** 
Payback voucher amount capacity is locked on a capacity of MTH tokens staked in Monetha voucher contract

### Forking the Reputation Layer

Our implementation can be forked or similar ones can be built.

The solutions are in favour of our implementation:

* Provide a valuable and useful solution;
* Paybacks and discounts that are maintained and guaranteed by Monetha.