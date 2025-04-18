# Custom Fee Schedule

## Overview

The Hedera Token Service (HTS) enables token issuers to set up to 10 automated, protocol-enforced custom fees per token. These fees apply automatically during token transfer transactions, facilitating revenue generation, royalty distribution, or incentive structures without the need for additional coding or smart contracts. This page will walk you through each token and how it operates and interacts with the network.&#x20;

***

## Types of Custom Fees&#x20;

HTS supports three types of custom fees, allowing token issuers to design flexible fee structures that align with various use cases and business models:

**Fixed Fee:** Paid by the _sender_ of the fungible or non-fungible tokens. A fixed fee transfers a set amount to a fee collector account each time a token is transferred, independent of the transfer size. This fee can be collected in HBAR or another Hedera token but not in NFTs.

**Fractional Fee:** Take a specific portion of the transferred fungible tokens, with optional minimum and maximum limits. The token _receiver_ (fee collector account) pays these fees by default. However, if [`net_of_transfers`](https://docs.hedera.com/hedera/sdks-and-apis/hedera-api/token-service/customfees/fractionalfee) is set to true, the sender pays the fees and the receiver collects the full token transfer amount. If this field is set to false, the receiver pays for the token custom fees and gets the remaining token balance.

**Royalty Fee:** Paid by the _receiver_ account that is exchanging the fungible value for the NFT. When the NFT sender does not receive any fungible value, the [fallback fee](https://docs.hedera.com/hedera/support-and-community/glossary#fallback-fee) is charged to the NFT receiver.

📝 **Example Breakdown:**

<table><thead><tr><th width="161.27862548828125">Fee Type</th><th>Who Pays?</th><th>Payment Type</th></tr></thead><tbody><tr><td><strong>Fixed Fee</strong></td><td>Sender</td><td>HBAR or HTS FT</td></tr><tr><td><strong>Fractional Fee</strong></td><td>Sender (or recipient if configured)</td><td>HTS FT</td></tr><tr><td><strong>Royalty Fee</strong></td><td>NFT Buyer (if exchanged for FTs)</td><td>HBAR or HTS FT</td></tr><tr><td><strong>Royalty Fallback</strong></td><td>NFT Receiver (if no fungible value received)</td><td>Fixed fee in HBAR</td></tr></tbody></table>

{% hint style="info" %}
#### **Note**

In addition to the custom token fee payment, the sender account must pay for the token transfer transaction fee in HBAR. The "[_Payment of Custom Fees & Transaction Fees in HBAR_](https://docs.hedera.com/hedera/sdks-and-apis/sdks/token-service/custom-token-fees#payment-of-custom-fees-vs.-transaction-fees-in-hbar)_"_ section below covers the distinction between custom fees and transaction fees.
{% endhint %}

### How They Work

### 1️⃣ Fixed Fee

A fixed fee is a predetermined amount collected by a designated fee collector each time a token transfer occurs. This fee is independent of the transfer volume and is always paid by the sender. It can be collected in HBAR or an HTS fungible token (FT), but NFTs cannot be used as a fee payment type. Fixed fees can be applied to both fungible and non-fungible tokens, ensuring consistent fee collection per transaction.

### 2️⃣ Fractional Fee

A fractional fee deducts a percentage of the transferred amount and credits it to a designated fee collector account. This fee is only applicable to HTS fungible tokens (FTs) and must be ≤1 while staying within the fractional range of a 64-bit signed integer. By default, the recipient covers the fee, meaning the deducted amount is taken from what they receive. However, if `net_of_transfers` is set to true, the sender pays the fee, ensuring the recipient gets the full intended amount.

In essence, a fractional fee deducts a percentage of the transferred amount, with configurable payer responsibility (sender or recipient) and optional minimum and maximum limits to enforce boundaries on the deducted amount.

{% hint style="info" %}
#### Example

* A 1% fractional fee applies to token transfers, with a minimum of 1 token and a maximum of 50 tokens.
* A 1,000-token transfer incurs a 10-token fee (1% of 1000).
* A 20-token transfer incurs the minimum fee of 1 token since 1% of 20 is less than 1.
{% endhint %}

### 3️⃣ Royalty Fee (NFTs)

A royalty fee applies when an NFT is transferred in exchange for fungible tokens (e.g. when an NFT is sold). The receiver pays the royalty fee, which is deducted as a percentage of the fungible amount exchanged. If no fungible tokens are involved in the transfer (i.e., the NFT is gifted or moved without payment), a fixed [fallback fee](https://docs.hedera.com/hedera/support-and-community/glossary#fallback-fee) is instead charged to the NFT recipient.

Key Points:

* Royalty fees apply only to non-fungible tokens (NFTs).
* Fees are paid in HBAR or an HTS fungible token.
* If the transfer includes no fungible value, a fallback fee is imposed.

{% hint style="info" %}
#### Example

An NFT with a 5% royalty fee on a 1,000 HBAR sale results in a 50 HBAR fee to the fee collector. If the NFT is transferred without a sale, a fixed fallback fee (e.g., 10 HBAR) is charged to the recipient.
{% endhint %}

{% hint style="warning" %}
#### 🔔 **Note**

Royalty fees function as a convenience feature, but the network cannot enforce royalties if users split the NFT exchange into separate transactions. To ensure proper application, the NFT sender and receiver must both sign a single `CryptoTransfer` transaction. There is an ongoing HIP discussion about expanding automatic royalty collection.
{% endhint %}

***

## Payment of Custom Fees vs. Transaction Fees in HBAR <a href="#payment-of-custom-fees-vs.-transaction-fees-in-hbar" id="payment-of-custom-fees-vs.-transaction-fees-in-hbar"></a>

Understanding the difference between custom fees and standard transaction fees in HBAR is crucial for token issuers and developers working with Hedera.

* **Custom fees** are designed to enforce complex fee structures, such as royalties and fractional ownership. These fees can be fixed, fractional, or royalty-based and are usually paid in the token being transferred, although other Hedera tokens or HBAR can also be used. You can configure up to 10 custom fees to be automatically disbursed to designated fee collector accounts.
* On the other hand, **transaction fees** in HBAR serve a different purpose: they compensate network nodes for processing transactions. These fees are uniform across all transaction types and are paid exclusively in HBAR. Unlike custom fees, which can be configured by the user, transaction fees are fixed by the network.

### **Key Differences**

The table below summarizes the key differences between custom fees and transaction fees.

<table><thead><tr><th width="164">Feature</th><th>Custom Fees</th><th>Transaction Fees</th></tr></thead><tbody><tr><td><strong>Purpose</strong></td><td>Enforce token-specific fee structures (e.g., royalties, taxes)</td><td>Compensate network nodes for transaction processing</td></tr><tr><td><strong>Who Collects?</strong></td><td>Designated fee collector(s)</td><td>Hedera network nodes</td></tr><tr><td><strong>Currency</strong></td><td>HBAR or HTS fungible tokens</td><td>HBAR only</td></tr><tr><td><strong>Configurability</strong></td><td>Fully configurable by token issuer</td><td>Fixed by the network</td></tr></tbody></table>

***

### **Fee Exemptions**

Fee collector accounts can be exempt from paying custom fees. To enable this, you need to set the exemption during the creation of the custom fees ([HIP-573](https://hips.hedera.com/hip/hip-573)). If not enabled, custom fees will only be exempt for an account if that account is set as a fee collector.

### **Limits and Constraints**

When it comes to setting custom fees, there are a few limits and constraints to keep in mind:

* First, fees cannot be set to a negative value.
* Each token can have up to 10 different custom fees.
* Additionally, the treasury account for a given token is automatically exempt from paying these custom transaction fees.
* The system also permits, at most, two "levels" of custom fees. That means a token being transferred might require fees in another token that also has its own fee schedule; however, this can only be nested two layers deep to prevent excessive complexity.

***

## Additional Resources

* [**NFT Royalty Fees: Everything You Need To Know**](https://hedera.com/blog/nft-royalty-fees-hedera-hashgraph)
* [**Hedera Token Service: NFT Token Keys Edge Cases**](https://hedera.com/blog/hedera-token-service-nft-token-keys-edge-cases)
