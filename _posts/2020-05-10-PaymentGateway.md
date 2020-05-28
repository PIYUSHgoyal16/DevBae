---
layout: post
author: "Piyush Goyal"
author_url: https://github.com/PIYUSHgoyal16
title: "Tokenisation"
subtitle: "Learn How Tokenisation and Payment Gateways work"
bg_url: "https://user-images.githubusercontent.com/43112419/83114167-4217ce00-a0e6-11ea-91fb-0104ce1457aa.jpg"
tags: [home]
---


Tokenization is a data security mechanism that protects sensitive information against
hacking and identity thefts.
All businesses, whether brick-and-mortar establishments, ecommerce portals, or mobile
payment apps, have an obligation to protect customers’ payment card data. The Payment
Card Industry Data Security Standards (PCI DSS) provide guidelines for businesses to
secure the sensitive data used in payment transactions. One of the primary
recommendations of PCI DSS is tokenization.

__Table of Contents__


1. **[What is tokenization?](#understanding)**
2. **[How mobile payments work?](#payments)**
3. **[How tokenization works](#works)**
4. **[Tokensiation](#software)**
5. **[Functional requirements for a tokenization system](#functional)**
6. **[Token Generator](#generator)**
7. **[Token Datastore](#datastore)**
8. **[Cryptographic Key Management](#cryp)**

<br>


<h2 id="understanding"> What is tokenization? </h2>

PCI DSS defines tokenization as "a process by which the primary account number (PAN) is
replaced with a surrogate value called a token”.

To understand tokenization, first let’s discuss how mobile payments work.

<h2 id="payments">How mobile payments work?</h2>
Mobile payment systems terminology:
<ul>
<li>Customer: Card holder who initiates the payment via credit card</li>
<li>Merchant: Accepts card payments</li>
<li>Card schemes: Networks that connect card holders to merchants all over the world
and facilitate the use of the same card across geographies and environments. They
do not actually hold any funds, but provide the communication infrastructure
between various parties. Example: Visa and MasterCard</li>
<li>Acquiring bank: Takes responsibility for the merchant to being able to accept card
payments.</li>
<li>Issuing bank: Takes responsibility for the trading or credit worthiness of the card
holder.
</li>
</ul> 
2
Figure 1 shows the typical transaction workflow for a card payment.

![image alt ><](https://user-images.githubusercontent.com/43112419/83131538-97aba500-a0fd-11ea-890b-d3fa5deb38de.png)
Figure 1: Payment transaction without tokenization

The steps illustrated in Figure 2 are:
<ol>
<li>Customer uses card to begin the payment and presents their information to the
mobile app.</li>
<li>The customer information goes to the acquiring bank.</li>
<li>The acquiring bank passes on the information to the card scheme.</li>
<li>The card scheme passes the information to the issuing bank.</li>
<li>The issuing bank checks if the customer has enough credit to cover the purchase,
was the card stolen, and checks other safety aspects.</li>
<li>If the security checks pass, then the issuing bank responds with a go-ahead
message to the card scheme.</li>
<li>The card scheme passes that message to the acquiring bank.</li>
<li>Acquiring bank routes the message to the customer through the mobile app
interface.</li>
<li>At the end of the day, the transaction is settled, wherein the issuing bank gives the
funds to the acquiring bank through the card scheme network.</li>
<li>The acquiring bank transfers the funds to the merchant’s account.</li>
</ol> 

<h2 id="works">How tokenization works</h2>
The transaction workflow described above is susceptible to security breaches – hackers
can breach the communication networks carrying the customer information and can get
access to sensitive customer information such as the primary card number (PAN), expiry
date of the card, and so on. Tokenization is used as a safeguard against the hack.
Figure 2 depicts a card data transaction that employs tokenization:

![image alt ><](https://user-images.githubusercontent.com/43112419/83131983-55cf2e80-a0fe-11ea-8d80-0cf64048928e.png)
Figure 2: Payment transaction with tokenization

The steps illustrated in Figure 3 are:
<ol>
<li>The application collects or generates a piece of sensitive data.
<li>The data is immediately sent to the tokenization server — it is not stored locally.
<li>The payment service provider stores the card number and expiry date at an
incredibly secure PCI-DSS level 1 compliance system. The payment service
provider creates a token corresponding to the actual information. The sensitive
value and the token are stored in a highly secure and restricted database (usually
encrypted).
<li>The tokenization server returns the token to the application. The application stores
the token, rather than the original value. The token is used for most transactions
with the application.
<li>When a transaction is initiated using the application, the application uses the token
to proceed with the transaction. The merchant application sends the token to the
acquiring bank.
<li>The token is passed to the card schemes.</li>
<li>The card schemes pass the token to the issuing bank.</li>
<li>The issuing bank validates the token with the Tokenization Server.</li>
<li>The issuing bank checks if the customer has enough credit to cover the purchase,
was the card stolen, and checks other safety aspects.</li>
<li>If the security checks pass, then the issuing bank responds with a go-ahead
message to the card scheme.</li>
<li>The card scheme passes that message to the acquiring bank.</li>
<li>Acquiring bank routes the message to the customer through the mobile app
interface.</li> 
<li>At the end of the day, the transaction is settled, wherein the issuing bank gives the
funds to the acquiring bank through the card scheme network.</li>
<li>The acquiring bank transfers the funds to the merchant’s account.</li>
Thus, if the merchant server is hacked, the information has no value. The original
information is outside all the back and forth in a secured data vault.</li>
</ol>

<h2 id="functional">Functional requirements for a tokenization system</h2>
The functional requirements for a tokenization system are:
<ul>
<li>The system must be logically isolated from data processing systems and
applications.</li>
<li>The datastore used for storing the tokens and PANs should comply with PCI DSS
standards.</li>
<li>The token generation system should be impervious to direct attacks, cryptographic
analysis, and brute force attempts.</li>
<li>Possible token formats: Tokens can consist of random alphabetic and numeric
characters, only numeric characters, or may retain the first and last four digits of
the original PAN with the middle digits replaced by random characters.</li>
</ul>

<h2 id="generator">Token Generator</h2>
Token generation is the process or method of creating a token. Three common methods of
token generation are:
<ul>
<li>Random number generation:
In this method, PAN is replaced with a random numeric or alphanumeric value. The
surrogate value is not numerically derived from PAN. Hence the token cannot be
reverse-engineered to determine the original PAN value. Thus this method is a
highly secure way to generate tokens.</li>
<li>Encryption:
In this method, the PAN is encrypted to generate the corresponding token. The
encryption is performed using a strong cryptographic algorithm and encryption
key.</li>
<li>Hash function:
In this method, the token is generated by running a one-way non-reversible hash
function on the original PAN value.</li>
</ul>

<h2 id="datastore">Token Datastore</h2>
The generated tokens are stored along with the corresponding original PAN data in PCI
DSS Level 1 secured databases with highly limited access. The data is encrypted to ensure
sensitive data is safeguarded against stolen media or security breach. The datastore is the
only point of contact with the applications requesting the token for a card transaction.

<h2 id="cryp">Cryptographic Key Management</h2>
Cryptographic key management creates and manages the cryptographic keys used for
encrypting the data in the datastore, as well as the keys used in the generation of token (if
any).
