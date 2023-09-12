# Hedera's EVM Equivalence: Goals and Exceptions

Hedera aims to achieve [Ethereum Virtual Machine (EVM)](../../support-and-community/glossary.md#ethereum-virtual-machine-evm) equivalence to enhance the programmability and adoption of the network. This page outlines Hedera's EVM equivalence goals and highlights the key differences and exceptions between Hedera and [Ethereum](../../support-and-community/glossary.md#ethereum).

## Hedera's EVM Equivalence Goals

Hedera's EVM equivalence strategy is designed to empower developers by facilitating the execution of EVM smart contracts more rapidly and cost-effectively while ensuring seamless integration of Hedera’s native services. The key goals include:

#### Robust Application Programmability

Hedera aims to enable a wider range of application programmability without sacrificing its unique value. This means allowing developers to build a diverse range of applications on the Hedera network, from [decentralized finance (DeFi)](../../support-and-community/glossary.md#decentralized-finance-defi) applications to supply chain solutions, while still benefiting from the unique features of Hedera, such as its speed, security, and fairness.

#### Seamless Integration

Hedera plans to integrate its native services with the EVM seamlessly. This will allow developers to execute [smart contracts](../../support-and-community/glossary.md#smart-contract) more rapidly and cost-effectively. It will also make it easier for developers to use Hedera's services, as they can use the same tools and languages they are already familiar with from the Ethereum ecosystem.

#### Leverage Existing Expertise

Hedera's strategy involves leveraging the [HyperLedger Besu EVM](hyperledger-besu-evm.md) client, which allows developers to capitalize on their existing Solidity expertise. This means that developers who are already familiar with [Solidity](../../support-and-community/glossary.md#solidity) and the EVM can easily transition to developing on Hedera, taking advantage of its unique strengths such as faster transactions, low and fixed fees, fair transaction ordering, immediate finality, and heightened security.

#### Smooth Transition

Hedera aims to provide a seamless transition for developers already working with EVM-based platforms. This means making it easy for developers to move their existing applications, smart contracts, and other digital assets to the Hedera network. This goal is aimed at encouraging the adoption of the Hedera network by reducing the barriers to entry for developers.

#### Support for EVM Tools

Hedera plans to support familiar [EVM tools](../../#evm-compatible-tools), libraries, and environments. This includes providing support for popular development environments like [Truffle](https://trufflesuite.com/) and [HardHat](https://hardhat.org/), as well as libraries like [Web3js](https://web3js.readthedocs.io/en/v1.10.0/) and [EthersJS](https://ethers.org/). The network also supports additional tools like [MetaMask](../../support-and-community/glossary.md#metamask), [The Graph](https://thegraph.com/), and [Remix](https://remix.ethereum.org/#lang=en\&optimize=false\&runs=200\&evmVersion=null). This goal is aimed at making it easier for developers to build on Hedera, as they can use the same tools they are already familiar with from the Ethereum ecosystem.

#### Community Engagement

Hedera encourages developers to contribute to its EVM development and join the community discussions in the official [Hedera Discord](https://www.hedera.com/discord). This goal is aimed at fostering a vibrant and engaged developer community, which is crucial for the long-term success and sustainability of the Hedera network.

{% hint style="info" %}
🔔 Detailed [blog post](https://hedera.com/blog/evm-equivalence-unveiling-hederas-strategy-for-enhanced-programmability-and-network-adoption) of Hedera's EVM Equivalence strategy.&#x20;
{% endhint %}

## Key Differences and Exceptions between Hedera and Ethereum

While Hedera aims to achieve EVM equivalence, there are several exceptions and key differences to be aware of:

### Transaction and Network Differences

| Function                    | Hedera                                                                                   | Ethereum                                     |
| --------------------------- | ---------------------------------------------------------------------------------------- | -------------------------------------------- |
| Transaction Size Limit      | 6kb                                                                                      | No limit                                     |
| API Endpoints               | Consensus and mirror nodes do not provide Ethereum RPC API endpoints                     | Ethereum RPC API endpoints available         |
| Data Return on Static Calls | Data retrieval must be done through the relay                                            | Data returned directly                       |
| Query Costs                 | Not free, can use mirror node for free queries                                           | Free read-only calls                         |
| Transaction Throttling      | [Transactions may be throttled by gas limits](gas-and-fees.md#gas-per-second-throttling) | Transactions pending until future submission |
| Mempools                    | No mempools                                                                              | Mempools available                           |

### Authorization and Account Differences

| Function                 | Hedera                                                        | Ethereum                              |
| ------------------------ | ------------------------------------------------------------- | ------------------------------------- |
| Authorization Signatures | Used for transaction authorization outside of smart contracts | Typically used within smart contracts |
| Special System Accounts  | Available with unique properties                              | Not available                         |
| Non-ECDSA Accounts       | <p>Non-ECDSA accounts (such as ED or multi -key) are supported by Hedera but not supported on Ethereum.</p><p>ECDSA Accounts are fully compatible</p>            | Fully compatible                      |
| Account Deletion         | Possible                                                      | Not possible                          |

### Token and Fee Differences

| Function           | Hedera                                                                                                       | Ethereum                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------- |
| Native Tokens      | Supports native tokens in addition to [ERC-20 and ERC-721 token standards](supported-erc-token-standards.md) | All ERC token standards but primarily ERC-20 and ERC-721 tokens. |
| Contract Lifecycle | Contract entities can expire, rent fees may apply                                                            | No expiration or rent fees                                       |
| Gas Fees           | Charges at least 80% of gas fees regardless of transaction outcome                                           | Gas fees depend on transaction outcome                           |
| Fee Structure      | [Complex with two different gas prices](gas-and-fees.md)                                                     | Single gas price                                                 |
| Token Association  | [Concept of token association ](../../sdks-and-apis/sdks/readme-1/associate-tokens-to-an-account.md)         | No concept of token association                                  |

### Keys and Other Differences

| Function                     | Hedera                                                                                            | Ethereum                              |
| ---------------------------- | ------------------------------------------------------------------------------------------------- | ------------------------------------- |
| Keys for Token Functionality | Keys control access to token functionality (KYC, freeze, wipe, supply, fee, and pause)            | No equivalent functionality           |
| Precheck Failures            | [Multiple precheck failure reasons](../../sdks-and-apis/hedera-api/miscellaneous/responsecode.md) | Typically single failure reason       |
| Communication                | Requires communication with both consensus and mirror nodes                                       | Direct communication with nodes       |
| HBAR Decimal Precision       | 8 or 18[ (varies across Hedera APIs)](../../sdks-and-apis/sdks/hbars.md#hbar-decimal-places)      | Consistent 18 point decimal precision |
