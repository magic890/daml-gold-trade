# DAML Gold Trade / Asset Pledging - Use Case

In the Middle East gold trade and pledging of gold assets to gain profit is a popular practice. Raw gold, bullion or money are pledged to a counterparty (i.e., lender) which in exchange will provide an interest payment.

The lender will work with the asset in custody, gaining his profit. As an example, a lender might pledge an asset to a shop to sell on the market with a markup.

At the end of the pledge, the asset will be returned to the previous owner along with the interest-earned payment.

## Overview

In this demonstrating scenario, we want to model the custody exchange of assets along with the interests and payments.

## Workflow

In this scenario participants that own an `Asset` (e.g., gold), can decide to pledge the asset to another party and ask for an interest in return. This action is performed exercising the `PledgeAssetTo` of the contract and generates an `AssetPledgeProposal` contract. This destroys the previous `Asset` contract to lock the double pledging of the same asset.

If the pledge proposal is accepted by the other party, exercising the choice `Accept`, the asset custody is transferred. A new `Asset` contract is created with a new entry in the attributes `pledgedTo`, `pledgedInDate` and `pledgedBearingInterest` (i.e., referred to as "Asset (Pledged)" in the diagram below).

The custody chain and pledges history in the `Asset` enable all the participants to see the changes and interest rates applied. The transparency achieved increases confidence and incentivizes all parties to act with integrity and honesty.

When an asset pledged is returned, the ownership is transferred back to the previous pledger or issuer. A `Payment` is generated from the lender to the borrower corresponding to the interest accrued from the pledge.

![daml-gold-trade-model](https://user-images.githubusercontent.com/1352670/211362768-5be841c2-1548-4998-9fa5-3df9e1eb9ce1.png)

## Roles and Responsibilities

| Role | Responsabilities |
|---|---|
| Issuer | Participant that has the initial ownership of the asset. <br> He can decide to propose a pledge to another participant in the market, definining the conditions (i.e., bearing interest rate). |
| Lender | Participant that accepts a pledge of an asset. <br> He maintains custody of the asset and agrees to pay to the asset borrower an interest matured at the end of the pledge. <br> The lender (current owner) can pledge the asset to other participants that do not have already the same asset in custody. This avoids double pledging to the same participant or issuer of the asset. |

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
| `proposalReceiver: Party` | Targeted participants for the asset pledge. |
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

