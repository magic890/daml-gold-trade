# DAML Gold Trade / Asset Pledging - Use Case

In Middle East gold trade and pledging of gold assets in order gain profit is a popular practice. Raw gold, buillions or money are pledged to a counterparty (i.e., lender) which in exchange will provide an interest payment.

The lender will work with the asset in custody, gaining his own profit. As an example, a lender might pledge an asset to a shop to sell on the market with a markup.

At the end of the pledge, the asset will be returned to the previous owner along with the intrest earned payment.

## Overview

In this demonstrating scenario we want to model the custody exchange of assets along with the intrests and payments.

## Workflow

In this scenario partecipants that own an `Asset` (e.g. gold), can decide to pledge the asset to another party and ask for an interest in return. This action is performed exercising the `PledgeAssetTo` of the contract and generates an `AssetPledgeProposal` contract. This destroies the previous `Asset` contract in order to lock the double pleding of the same asset.

If the pledge proposal is accepted from the other party, exercising the choice `Accept`, the asset custody is transfered. A new `Asset` contract is created with a new entry in the attributes `pledgedTo`, `pledgedInDate` and `pledgedBearingInterest` (i.e., referred as "Asset (Pledged)" in the diagram below).

The custody chain and pledges history in the `Asset` enable all the partecipants to see the changes and interest rates applied. The  transparency achieved increases confidence and incentivize all parties to act with integrity and honesty.

When an asset pledged is returned, the ownership is transferred back to the previous pledger or issuer. A `Payment` is generated from the lender to the borrower corresponding to the interest accrued from the pledge.

![daml-gold-trade-model](https://user-images.githubusercontent.com/1352670/211335627-6380e069-9885-4f75-99c8-48a3e2cd4fd4.png)

## Roles and Responsibilities

| Role | Responsabilities |
|---|---|
| Issuer | Participant that has the initial ownership of the asset. <br> He can decide to propose a pledge to another participant in the market, definining the conditions (i.e., bearing interest rate). |
| Lender | Participant that accepts a pledge of an asset. <br> He maintain custody of the asset and agrees to pay to the asset borrower an interest matured at the end of the pledge. <br> Lenders can pledge the asset to other partecipants that do not have already the same asset in custody. This implies that double pledging to the same participant or issuer of the asset is not allowed. |

## Contracts Details

### Asset

| Attribute | Description |
|---|---|
| `issuer: Party` | Initial owner of the asset. |
| `pledgedTo: [Party]` | Chain of custody identifying the lenders. |
| `pledgedInDate: [Date]` | Identifies the beginning of the pledges. |
| `pledgedBearingInterest: [Decimal]` | Describes the bearing interest rates of each pledge. |
| `name: Text` | Description of the asset. |
| `value: Decimal` | Nominal value of the asset. |
| `currency: Text` | Currency of the nominal value. |

### AssetPledgeProposal

| Attribute | Description |
|---|---|
| `asset: Asset` | Asset details. |
| `proposalReceiver: Party` | Targeted partecipants for the asset pledge. |
| `proposalDate: Date` | Proposed beginning of the pledge. |
| `proposalBearingInterest: Decimal` | Proposed interest rate for the pledge. |

### Payment

| Attribute | Description |
|---|---|
| `date: Date` | Date of the payment. |
| `sender: Party` | Participant sending the payment.<br>Usually the lender paying the interest matured to the previous asset custodian. |
| `receiver: Party` | Participant receiving the payment. |
| `value: Decimal` | Nominal value of the payment. |
| `currency: Text` | Currency of the nominal value. |

