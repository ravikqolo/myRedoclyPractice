# Qolo Platform Structure

The diagram below summarizes the Qolo platform structure and the relation between its various components.

![](RackMultipart20220324-4-btw0jy_html_7add8aeed6efeea4.png)

The Qolo platform structure allows for flexible implementation of your unique program.

## Qolo Component Definitions

**Person:** A person on the Qolo Platform can be an individual or a company. Qolo platform sets up all end users, cardholders, clients, program partners and any third-party service providers as a &#39;Person&#39; on the platform. [API reference](https://devdocs.qolopay.com/openapi/qoloreference/tag/Persons)

**Outside Instrument:** Outside Instruments are used to identify external bank account or debit/credit cards. Registered Persons on Qolo platform can move funds in either direction between their Qolo account and registered Outside Instruments, based on their program configuration. [API reference](https://devdocs.qolopay.com/openapi/qoloreference/operation/CreateInstrument)

**Wallet:** A wallet, assigned to a person, holds various Qolo accounts and cards corresponding to the person. A person may have one or multiple wallets on the platform. [API reference](https://devdocs.qolopay.com/openapi/qoloreference/operation/CreateWallet)

**Qolo Account:** An account is a currency account issued and maintained at Qolo. Accounts are encapsulated in a person&#39;s wallet. [API reference](https://devdocs.qolopay.com/openapi/qoloreference/operation/CreateAccount)

**Qolo Issued DDA:** A Qolo Account may have one or multiple DDAs associated with it. Users can add funds to their Qolo Accounts by sending funds to these associated DDAs.

**Qolo Card:** A card is a payment device that enables a user to conduct transactions at merchants, either online or offline. Qolo Cards are assigned to a person&#39;s wallet and access funds from Qolo Accounts in the same wallet. [API reference](https://devdocs.qolopay.com/openapi/qoloreference/operation/CreateCard)




#

# Person

A &#39;Person&#39; on the Qolo Platform can be an individual or a company. Based on the program requirements, a Person may be associated to a wallet and account.

The Person on Qolo platform is represented by &#39;person&#39; object. The direct way to create a Person on Qolo platform is to use the &#39;Create Person&#39; API. Clients can choose to set the &#39;create\_account&#39; flag in the API request to also create a wallet and account(s) for the Person. [API reference](https://devdocs.qolopay.com/openapi/qoloreference/operation/CreatePerson)

## Person Verification / KYC

Regulations such as the Bank Secrecy Act/Anti-Money Laundering law (BSA/AML) require Qolo to form a reasonable confidence on the identity of each customer. This is required to control suspicious and/or fraudulent activity on the platform.

Some Programs require Persons to be verified before opening their account, or giving them access to certain functions. To perform a Person Verification, Qolo requires the following information:

1.
### For Natural Persons

- Legal name (first and last name);
- Complete residential address;
- Official government ID and the ID type.
- Date and place of birth.

### For Legal Persons (Currently handled offline)

- Name, legal form, status and proof of incorporation of the legal person;
- Permanent address of the principal place of the legal person&#39;s activities;
- Official identification number (company registration number, tax identification number);
- Mailing and registered address of legal person;
- License or legal opinion from a law firm located in each country where business activity will occur or be offered to cardholders.
- Any other documents to complete verification.

Qolo would verify the identity of the Person through data collected, but may also request for documentary evidence or additional information. [API reference](https://devdocs.qolopay.com/openapi/qoloreference/operation/PerformKYC)

## Person Status

Each Person has a status, &#39;person\_status&#39; that controls the Person&#39;s access to various functions and capabilities. Qolo may change status of a Person based on Verification Status or Account Usage. [API reference](https://devdocs.qolopay.com/openapi/qoloreference/operation/UpdatePerson)

The different Person status and access level on Qolo platform are:

| Person Status | Access Level |
| --- | --- |
| CLEARED | No Restriction |
| INCOMPLETE, UNVERIFIED, ANONYMOUS | Limited Restriction |
| SANCTIONED, PEP, NEGLIST | Restricted |

# Card

A card is a payment device attached to a Person Account on Qolo platform. It accesses the funds in the Account and enables the Person to transact at merchants or withdraw funds from an ATM. Cards can be physical or virtual and may be tokenized for use in digital wallets like Apple Pay or Samsung Pay.

Card parameters are defined under a Qolo Program and includes –

1. _Card Form -_ Physical, Virtual, Tokenized
2. _Card Issuance Type -_ Single Issuance, Bulk Issuance, Instant Issue
3. _Card Scheme -_ Visa, MasterCard, Discover
4. _Card BIN / Sub BIN_
5. _Card Currencies_
6. _Single Load or Reloadable_

Card on Qolo platform are represented by &#39;card&#39; object and comprises of all the card information.

A typical Card lifecycle in a card based Program may be depicted as:

![Shape2](RackMultipart20220324-4-btw0jy_html_4984936c79e099e6.gif)

## Create Card

Card can be created on Qolo platform using [&#39;Create Card&#39; API](https://devdocs.qolopay.com/openapi/qoloreference/operation/CreateCard). Clients must provide Qolo Program ID and Person details in the API.

1. _For an existing Person on Qolo Platform:_ Clients must provide the Person GUID in the Create Card API.
2. _For a new/unregistered Person:_ Clients must provide the entire Person object in the Create Card API. The API end point creates a Person (as cardholder) with a wallet, account and a card, and associates the card to the Person&#39;s wallet.

Some programs may have a limitation on Card creation based on the cardholder (Person) verification status. For such programs, Card creation may be altogether denied; or restricted to a limited capability card for unverified cardholders (Persons).

## Card Status

A card may go through various status over its lifecycle. Each Card status defines a card state and controls the actions that can be performed on the card. [API reference](https://devdocs.qolopay.com/openapi/qoloreference/operation/StatusCard)

| Card Status | Description | Transactions Allowed |
| --- | --- | --- |
| PENDING | New Card. | Load only |
| ACTIVE | Card is active and can be used for transactions. | All Transactions allowed |
| SUSPEND | Temporarily block card usage. | Load &amp; Recurring Txns |
| LOST, STOLEN | Card is lost or stolen. Card must be replaced. | Load only |
| DAMAGED | Card is damaged. Card must be reissued. | Load and Online Txns |
| CLOSED | Card account closed permanently. | No Txns allowed |
| FCLOSED |
 | No Txns allowed |
| FORGET |
 |
 |

## Activate Card

A new card is created in &#39;Pending&#39; status and must be activated before it can be used for transactions at merchants or ATMs.

Clients must successfully identify the cardholder and confirm card possession before activating their card. Clients are advised to ask cardholders, the Card number, CVV and their Date of Birth (that was provided during card creation) to successfully verify the cardholder and their possession of the card. This may be done over an IVR, Web portal or a Mobile Application. On successful verification, clients can proceed to call [&#39;Activate Card&#39; API](https://devdocs.qolopay.com/openapi/qoloreference/operation/ActivateCard)to change the card status from &#39;Pending&#39; to &#39;Active&#39;.

For debit PIN enabled cards, clients must allow cardholders to set a PIN at the time of Card Activation. Clients can use [&#39;Set PIN](https://devdocs.qolopay.com/openapi/qoloreference/operation/SetPIN)&#39; and [&#39;Get PIN&#39; API](https://devdocs.qolopay.com/openapi/qoloreference/operation/GetPIN)end points to set or get the cardholder&#39;s debit PIN.

## Card Lost, Stolen or Damaged - Replace &amp; Reissue Cards

If a cardholder loses access to their card, or their card stops functioning; a new card must be issued to the cardholder.

1. If the card is lost or stolen, the original card must be moved to &#39;Lost&#39; or &#39;Stolen&#39; status and a new card must be issued with a new PAN / Card number. This is referred to as &#39;Card Replacement&#39;.
2. If the card is damaged or has stopped functioning, the original card must be moved to &#39;Damaged&#39; status, and a new card must be issued with the same PAN / Card number. This is referred to as &#39;Card Reissuance&#39;.

P.S. A replacement or reissued card still accesses the same account as the original card. Therefore, no fund transfer is required for the replacement or reissued card.

## Balance

A Card on Qolo platform doesn&#39;t have balance by itself. Instead, it accesses the balance of one or more Qolo Accounts.

In order to fetch the balance available through card, clients must call[&#39;Search Wallet&#39; API.](https://devdocs.qolopay.com/openapi/qoloreference/operation/SearchWallet) The &#39;Search Wallet&#39; API can take various input parameters (to search on) including the card proxy, and responds with the entire wallet object including accounts and their balances.

There are three types of balances on Qolo platform –

1. _Ledger balance:_ Ledger balance is the account balance. It is the total funds in the account, and includes amount authorized for a transaction, but not yet settled with the merchant.
2. _Available balance:_ Available balance is the &#39;open to buy&#39; balance of the account. It identifies the amount available to the user for transactions.
3. _Suspense balance:_ Suspense balance represents amounts pending credit to the account. These are funds still waiting to be cleared, before making them available to the account holder.

## Transaction History

Transaction History can be fetched for an account, card, or an external instrument using the [&#39;Search Transactions&#39; API.](https://devdocs.qolopay.com/openapi/qoloreference/operation/SearchTransaction)

Account GUID, Card Proxy/Number or External Instrument GUID can be sent in the API request to fetch transactions on the platform corresponding to the requested parameter.

Further information on the transaction can be retrieved using the [&#39;Retrieve Transaction&#39; API](https://devdocs.qolopay.com/openapi/qoloreference/operation/GetTransactionById) by sending the Transaction GUID in the request.

# External Account Transfers

External Accounts (referred to as Outside Instruments on Qolo platform) are external bank accounts or credit/debit cards. Transfer to or from an Outside Instrument can be achieved in 3 steps

## Create Person

The first step to initiating a transfer to / from an outside instrument is to create a person who would be the owner of the outside instrument. The[Create Person API](https://devdocs.qolopay.com/openapi/qoloreference/operation/CreatePerson)call would create a Person.

## Add Outside Instrument to Person

The next step is to create the outside instrument and assign it to an existing Person. The client must call the[Create Instrument API](https://devdocs.qolopay.com/openapi/qoloreference/operation/CreateInstrument) call to create/add an Outside Instrument to the Person. The instrument\_type must be chosen as &#39;BANKDDA&#39; for US bank accounts or as &#39;CARD&#39; for a credit/debit cards. Bank or card details must be provided in the account\_details as applicable.

## Transfer to/from Outside Instrument

In order to initiate the transfer to/from Outside Instrument, the client must call the[Create Transfer API](https://devdocs.qolopay.com/openapi/qoloreference/operation/CreateTransfer) and provide the outside\_instrument\_guid as either the source or destination based on the transfer type. Funds can be transferred to/from outside\_instrument\_guid to a Qolo account\_guid or card\_proxy.

The transfer\_type value must be chosen from one of the below options based on the type of transfer to/from outside instrument.

| Transfer Type | Description |
| --- | --- |
| ACCOUNTTOBANK | Transfer from Qolo Account to Outside Bank Account (ACH Out) |
| BANKTOACCOUNT | Transfer from Outside Bank Account to Qolo Account (ACH In) |
| CCDCTOACCOUNT | Transfer from Credit/Debit Card to Qolo Account (AFT) |
| ACCOUNTTOCCDC | Transfer from Qolo Account to Credit/Debit Card (OCT) |

# Recipes

## Create Card for a new Person with KYC

In this recipe you will attempt to create card for a new Person. The Person record will fail electronic verification and will require you to verify the Person using challenge questions. Card will be created after successful Person verification.

1. Create Card (Result: Fail, Person Created)
2. Retrieve Person KYC
3. Perform Person verification
4. Create Card

## Fetch Balance and Transaction History of a Card

In this recipe you will fetch the wallet GUID of a card using card proxy and then fetch the account balance of all the accounts in the wallet. You will also fetch the Transaction History using the card proxy.

1. Retrieve Card
2. Retrieve Wallet
3. Search Transactions

## Send Funds from Qolo Account in USD to US Bank Account

In this recipe you will add a US Bank Account to a Person Wallet as an Outside Instrument. You will then send funds from your USD Qolo Account to the US Bank Account.

1. Create Person
2. Create Instrument
3. Create Transfer

## Send Funds from Qolo Account in USD to Euro Bank Account

In this recipe you will add a Euro Bank Account to a Person Wallet as an Outside Instrument. You will then send funds from your USD Qolo Account to the Euro Bank Account. The transfer process will cover currency conversion quote and currency conversion process.

1. Create Person
2. Create Instrument
3. Quote
4. Create Transfer

# Dev Environment Setups

## Simulate Card Transaction (applicable only in Dev Environment)

Walk through the process of simulating card transaction in Dev Environment. The transactions can then be fetched using Transaction History API.

## Setup and Receive Webhook Notifications

Setup your system for Webhook Notifications, and receive Webhook notifications for the events and simulated transactions in Dev Environment.

## \*Interact Clients\* - Setup and Receive Authorization Messages

**Interact Clients Only**

Walk through the process of setting up your end point to receive Auth Messages. Auth Messages can be simulated using the Simulate Card Transaction methods. You would need to approve the Auth message in order to post the transaction to a card account.