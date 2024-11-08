# Gas and Fees

## Gas

When executing smart contracts, the EVM requires the amount of work paid in gas. The “work” includes computation, state transitions, and storage. Gas is the unit of measurement used to charge a fee per opcode executed by the EVM. Each opcode code has a defined gas cost. Gas reflects the cost necessary to pay for the computational resources used to process transactions.

### **Weibar**

The EVM returns gas information in Weibar (introduced in [HIP-410](https://hips.hedera.com/hip/hip-410)). One weibar is 10^-18th HBAR, which translates to 1 tinybar is 10^10 weibars. As noted in HIP-410, this maximizes compatibility with third-party tools that expect ether units to be operated on in fractions of 10^18, also known as a Wei.

### **Gas Schedule and Fees**

Gas fees paid for EVM transactions on Hedera can be composed of three different kinds of gas costs:

* Intrinsic Gas
* EVM opcode Gas
* Hedera System Contract Gas

<table><thead><tr><th width="150">Gas Fee Type</th><th>Description</th></tr></thead><tbody><tr><td><strong>Intrinsic Gas</strong></td><td><p>The minimum amount of gas required for every transaction submitted to the network starts with a base cost of 21,000 gas units. Each zero byte in the transaction payload adds 4 gas units, while each non-zero byte adds 16 gas units.</p><p></p><ul><li><p><strong>Intrinsic Gas Cost =</strong> </p><p><code>21000</code> </p><p>+ <code>4</code> <mark style="color:red;">*</mark> (number of zeros bytes in transaction payload) </p><p>+ <code>16</code> <mark style="color:red;">*</mark> (number of non-zeros bytes in transaction payload)</p></li></ul><p></p><p>If the gas provided is insufficient, the transaction will fail during precheck, and no record will be created.</p></td></tr><tr><td><strong>EVM Opcode Gas</strong></td><td><p>The gas required to execute the defined opcodes for the smart contract call.</p><ul><li><strong>Fixed Execution Cost</strong>: Each opcode has a fixed cost, measured in gas, to be paid upon execution. This cost is the same for all executions, though it may change with new hard forks.</li><li><strong>Dynamic Execution Cost</strong>: Some operations have variable costs based on parameters or context, such as whether the account or storage slot is "cold" (accessed for the first time in the transaction) or "warm" (already accessed).</li></ul><p>See the <a href="https://www.evm.codes/">reference</a> to learn about the specific costs per opcode and fork.</p></td></tr><tr><td><strong>Hedera System Contract Gas</strong></td><td><p>The required gas that is associated with a Hedera-defined transaction like using the Hedera Token Service system contract that allows you to burn (<code>TokenBurnTransaction</code>) or mint (<code>TokenMintTransaction</code>) a token. </p><p></p><p>System contract gas fees apply only when using a native Hedera service. These fees are calculated by converting the transaction cost in USD to gas using a set conversion rate, with a 20% surcharge added for overhead.</p><p></p><p></p><p><strong>Example</strong>: For a $0.10 transaction with a conversion rate of 1,000,000 gas per USD:</p><ul><li><strong>Base Gas Cost</strong> = 0.10 × 1,000,000 = 100,000 gas</li><li><strong>Total Gas Cost</strong> = 100,000 × 1.2 = 120,000 gas</li></ul><p></p><p>Example System Contracts:</p><ul><li>Hedera Token Service</li><li>Pseudo Random Number Generator (PRNG)</li><li>Exchange Rate</li></ul><p>See the detailed gas calculation <a href="https://github.com/hashgraph/hedera-services/blob/develop/hedera-node/docs/design/services/smart-contract-service/system-contract-gas-calc.md#system-contracts">reference</a>.</p></td></tr></tbody></table>

### Gas Limit

The gas limit is the maximum amount of gas you are willing to pay for an operation.

The current opcode gas fees are reflective of the [0.22 Hedera Service release](https://docs.hedera.com/hedera/networks/release-notes/services#v0.22).

| Operation                                                               | Cancun Cost (Gas)                              | Current Hedera (Gas)                           |
| ----------------------------------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- |
| Code deposit                                                            | 200 \* bytes                                   | 200 \* bytes                                   |
| <p><code>BALANCE</code><br>(cold account)</p>                           | 2600                                           | 2600                                           |
| <p><code>BALANCE</code><br>(warm account)</p>                           | 100                                            | 100                                            |
| `EXP`                                                                   | 10 + 50/byte                                   | 10 + 50/byte                                   |
| <p><code>EXTCODECOPY</code><br>(cold account)</p>                       | 2600 + Mem                                     | 2600 + Mem                                     |
| <p><code>EXTCODECOPY</code><br>(warm account)</p>                       | 100 + Mem                                      | 100 + Mem                                      |
| <p><code>EXTCODEHASH</code><br>(cold account)</p>                       | 2600                                           | 2600                                           |
| <p><code>EXTCODEHASH</code><br>(warm account)</p>                       | 100                                            | 100                                            |
| <p><code>EXTCODESIZE</code><br>(cold account)</p>                       | 2600                                           | 2600                                           |
| <p><code>EXTCODESIZE</code><br>(warm account)</p>                       | 100                                            | 100                                            |
| <p><code>LOG0, LOG1, LOG2,</code><br><code>LOG3, LOG4</code></p>        | <p>375 + 375*topics<br>+ data Mem</p>          | <p>375 + 375*topics<br>+ data Mem</p>          |
| <p><code>SLOAD</code><br>(cold slot)</p>                                | 2100                                           | 2100                                           |
| <p><code>SLOAD</code><br>(warm slot)</p>                                | 100                                            | 100                                            |
| <p><code>SSTORE</code><br>(new slot)</p>                                | 22,100                                         | 22,100                                         |
| <p><code>SSTORE</code><br>(existing slot,<br>cold access)</p>           | 2,900                                          | 2,900                                          |
| <p><code>SSTORE</code><br>(existing slot,<br>warm access)</p>           | 100                                            | 100                                            |
| <p><code>SSTORE</code><br>refund</p>                                    | As specified by the EVM                        | As specified by the EVM                        |
| <p><code>CALL</code> <em>et al</em>.<br>(cold recipient)</p>            | 2,600                                          | 2,600                                          |
| <p><code>CALL</code> <em>et al</em>.<br>(warm recipient)</p>            | 100                                            | 100                                            |
| <p><code>CALL</code> <em>et al</em>.<br>Hbar/Eth Transfer Surcharge</p> | 9,000                                          | 9,000                                          |
| <p><code>SELFDESTRUCT</code><br>(cold beneficiary)</p>                  | 2600                                           | 2600                                           |
| <p><code>SELFDESTRUCT</code><br>(warm beneficiary)</p>                  | 0                                              | 0                                              |
| `TSTORE`                                                                | 100                                            | 100                                            |
| `TLOAD`                                                                 | 100                                            | 100                                            |
| `MCOPY`                                                                 | 3 + 3\*words\_copied + memory\_expansion\_cost | 3 + 3\*words\_copied + memory\_expansion\_cost |

The terms 'warm' and 'cold' in the above table correspond with whether the account or storage slot has been read or written to within the current smart contract transaction, even within a child call frame.

'`CALL` _et al._' includes with limitation: `CALL`, `CALLCODE`, `DELEGATECALL`, and `STATICCALL`

Reference: [HIP-206](https://hips.hedera.com/hip/hip-206), [HIP-865](https://hips.hedera.com/hip/hip-865)

### Gas Per Second Throttling

Most EVM-compatible networks use a per-block gas limit to control resource allocation and limit block validation time, enabling miner nodes to produce new blocks quickly. While Hedera lacks blocks and miners, it must still manage resource use over time.

For smart contract transactions, gas is a more effective measure of transaction complexity than transaction count. To balance flexibility and resource management, Hedera mirrors Ethereum's approach by setting transaction limits based on gas consumption (for `ContractCreate`, `ContractCall`, and `ContractCallLocalQuery`), alongside per-transaction limits. This dual method enables precise regulation of smart contract executions.

The Hedera network has implemented a system gas throttle of **15 million gas per second** in the Hedera Service release [0.22](../../networks/release-notes/services.md#v0.22).&#x20;

### Gas Reservation and Unused Gas Refund

Hedera throttles transactions before consensus, and nodes limit the number of transactions they can submit to the network. Then, at consensus time, if the maximum number of transactions is exceeded, the excess transactions are not evaluated and are canceled with a busy state. Throttling by variable gas amounts provides challenges to this system, where the nodes only submit a share of their transaction limit.

To address this, throttling will be based on a two-tiered gas measuring system: pre-consensus and post-consensus. Pre-consensus throttling will use the `gasLimit` field specified in the transaction. Post-consensus will use the actual evaluated amount of gas the transaction consumes, allowing for dynamic adjustments in the system. It is impossible to know the _actual_ evaluated gas pre-consensus because the network state can directly impact the flow of the transaction, which is why pre-consensus uses the `gasLimit` field and will be referred to as the **gas reservation**.

Contract query requests are unique and bypass the consensus stage altogether. These requests are executed solely on the local node that receives them and only influence that specific node's precheck throttle. On the other hand, standard contract transactions go through both the precheck and consensus stages and are subject to both sets of throttle limits. The throttle limits for precheck and consensus may be set to different values.

In order to ensure that the transactions can execute properly, setting a higher gas reservation than will be consumed by execution is common. On Ethereum Mainnet, the entire reservation is charged to the account before execution, and the unused portion of the reservation is credited back. However, Ethereum utilizes a memory pool ([mempool](../../support-and-community/glossary.md#mempool)) and does transaction ordering at block production time, allowing the block limit to be based only on used and not reserved gas.

To help prevent over-reservation, Hedera restricts the amount of unused gas that can be refunded to a maximum of 20% of the original gas reservation. This effectively means users will be charged for at least 80% of their initial reservation, regardless of actual usage. This rule is designed to incentivize users to make more accurate gas estimates.

For example, if you initially reserve 5 million gas units for creating a smart contract but end up using only 2 million, Hedera will refund you 1 million gas units, or 20% of your initial reservation. This setup balances the network's resource management while incentivizing users to be as accurate as possible in their gas estimations.

### Maximum Gas Per Transaction

Each transaction on Hedera is capped by a per-transaction gas limit. If a transaction’s `gasLimit` exceeds this cap, it is rejected during precheck with the `INDIVIDUAL_TX_GAS_LIMIT_EXCEEDED` error and does not proceed to consensus. This gas metering approach ensures efficient resource use, preventing excessive consumption while allowing flexibility for larger, more complex smart contracts.

Gas throttle per contract call and contract create **15 million gas per second**.

Reference: [HIP-185](https://hips.hedera.com/hip/hip-185)
