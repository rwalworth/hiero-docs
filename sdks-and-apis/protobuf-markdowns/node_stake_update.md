# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/node_stake_update.proto](#services_node_stake_update-proto)
    - [NodeStake](#proto-NodeStake)
    - [NodeStakeUpdateTransactionBody](#proto-NodeStakeUpdateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_node_stake_update-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/node_stake_update.proto
# Node Stake Updates
A system-initiated (i.e. internal) transaction to update stake information
for nodes at the end of a staking period.

Note that staking rewards are not paid immediately. The rewards are
calculated, and the amount to be paid is reserved in the reward account,
at the end of each staking period. The actual recipient accounts are then
paid rewards when that account participates in any transaction that
changes staking information or the account balance. This reduces the
resources for calculating the staking rewards, quite dramatically,
and provides an incentive for account owners to engage with the network,
in at least a minor fashion, occasionally (typically annually).&lt;br/&gt;
The unexpected change in balances, however, can be surprising to the
account holder.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-NodeStake"></a>

### NodeStake
Staking information for one node at the end of a staking period.

This SHALL be one entry in a list reported at the end of each
full staking period.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| max_stake | [int64](#int64) |  | A limit to the amount of stake considered for consensus weight. &lt;p&gt; The amount of stake (whether accepting rewards or not) assigned to a node that exceeds this limit SHALL NOT be considered for consensus weight calculation.&lt;br/&gt; If stake to _reward_ for a node exceeds this threshold, then all accounts staking to that node SHALL receive a lower reward rate in proportion to the excess stake. |
| min_stake | [int64](#int64) |  | A minimum amount of HBAR staked to a node to receive rewards. &lt;p&gt; If the amount of stake (whether accepting rewards or not) assigned to a node at the start of a staking period is less than this threshold, then no rewards SHALL be paid to that node or to any accounts staking to that node. |
| node_id | [int64](#int64) |  | A node identifier.&lt;br/&gt; This value uniquely identifies this node within the network address book. |
| reward_rate | [int64](#int64) |  | The rate of rewards, in tinybar per HBAR, for the staking reward period that just ended. |
| stake | [int64](#int64) |  | A consensus weight assigned to this node for the next staking period. |
| stake_not_rewarded | [int64](#int64) |  | The total amount staked to this node, while declining rewards. &lt;p&gt; This SHALL be the total staked amount, in tinybar, that is staked to this node with the value of the `decline_reward` set.&lt;br/&gt; This value MUST be calculated at the beginning of the staking period. |
| stake_rewarded | [int64](#int64) |  | The total amount staked to this node, while accepting rewards. &lt;p&gt; This SHALL be the total staked amount, in tinybar, that is staked to this node with the value of the `decline_reward` not set.&lt;br/&gt; This value MUST be calculated at the beginning of the staking period. |






<a name="proto-NodeStakeUpdateTransactionBody"></a>

### NodeStakeUpdateTransactionBody
A system initiated transaction to update staking information.

This transaction SHALL be issued at the end of each staking period to update
node stakes and reward limits.&lt;br/&gt;
This transaction SHALL be a child of the first transaction to reach
consensus following the end of the previous staking period.&lt;br/&gt;
This transaction MUST NOT be sent by a client and SHALL be rejected if
received by any node.&lt;br/&gt;
This transaction SHALL be present in the record stream or block stream.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| end_of_staking_period | [Timestamp](#proto-Timestamp) |  | A timestamp indicating the end of the staking period. &lt;p&gt; This value SHALL be one nanosecond prior to midnight prior to the consensus time of the parent transaction. |
| node_stake | [NodeStake](#proto-NodeStake) | repeated | A list of `NodeStake` entries for each node at the beginning of the new staking period. &lt;p&gt; This list SHALL have one entry for each node participating in network consensus. |
| max_staking_reward_rate_per_hbar | [int64](#int64) |  | A maximum reward rate for this staking period. &lt;p&gt; This SHALL be a ratio of tinybar to HBAR.&lt;br/&gt; An account SHALL NOT receive a reward greater than the product of this ratio and the total number of HBAR staked by that account. |
| node_reward_fee_fraction | [Fraction](#proto-Fraction) |  | A fraction of network and service fees paid to the &#34;node&#34; reward account.&lt;br/&gt; The node staking rewards are paid from the designated reward account `0.0.801`, which receives a fraction of network and service fees for each transaction. This field is the value of that fraction for the last staking period. |
| staking_periods_stored | [int64](#int64) |  | A limit to the number of staking periods held for inactive accounts.&lt;br/&gt; This is the maximum number of trailing staking periods for which an account can collect staking rewards.&lt;br/&gt; #### Example If this value is 365 with a calendar day period, then each account must collect rewards at least once per calendar year to receive the full amount of staking rewards earned. &lt;p&gt; Staking rewards SHALL be stored in network state for no more than `staking_periods_stored` staking periods.&lt;br/&gt; Each account MUST participate in at least one transaction that affects its balance, staking, or staking metadata within this time limit to receive all available staking rewards. |
| staking_period | [int64](#int64) |  | A number of minutes representing a staking period.&lt;br/&gt; &lt;blockquote&gt;Note&lt;blockquote&gt; For the special case of `1440` minutes, periods are treated as calendar days aligned to midnight UTC, rather than repeating `1440` minute periods left-aligned at the epoch.&lt;/blockquote&gt;&lt;/blockquote&gt; |
| staking_reward_fee_fraction | [Fraction](#proto-Fraction) |  | A fraction of network and service fees paid to the &#34;general&#34; reward account.&lt;br/&gt; The general staking rewards are paid from the designated reward account `0.0.800`, which receives a fraction of network and service fees for each transaction. This field is the value of that fraction for the last staking period. |
| staking_start_threshold | [int64](#int64) |  | A minimum balance required to pay general staking rewards. &lt;p&gt; If the balance of the staking reward account `0.0.800` is below this threshold, staking rewards SHALL NOT be paid in full. |
| staking_reward_rate | [int64](#int64) |  | **Deprecated.** HIP-786 replaced this field with `max_total_reward`.&lt;br/&gt; This was a maximum total number of tinybars to be distributed as staking rewards in the staking period that just ended. |
| reserved_staking_rewards | [int64](#int64) |  | An amount reserved in the staking reward account.&lt;br/&gt; This is an amount &#34;reserved&#34; in the balance of account `0.0.800` that is already owed for pending rewards that were previously earned but have not yet been collected.&lt;br/&gt; This value is further detailed in HIP-786. |
| unreserved_staking_reward_balance | [int64](#int64) |  | An available, unreserved, amount in the staking reward account.&lt;br/&gt; This is the balance of the staking reward account `0.0.800` at the close of the staking period that just ended, after reduction for all &#34;reserved&#34; funds necessary to pay previously earned rewards.&lt;br/&gt; This value is further detailed in HIP-786. &lt;p&gt; This value SHALL be used to calculate the reward ratio according to the formula detailed in HIP-782. |
| reward_balance_threshold | [int64](#int64) |  | A minimum balance required for maximum staking rewards.&lt;br/&gt; This value is further detailed in HIP-786. The formula to calculate staking rewards is detailed in HIP-782. &lt;p&gt; The value of `unreserved_staking_reward_balance` MUST match or exceed the value of this field to support the maximum staking reward ratio.&lt;br/&gt; |
| max_stake_rewarded | [int64](#int64) |  | A maximum network-wide stake that can earn full rewards.&lt;br/&gt; If the network-wide stake, in tinybar, exceeds this value, then staking rewards must be reduced to maintain &#34;smooth&#34; reward adjustments as defined in HIP-782.&lt;br/&gt; This value is further detailed in HIP-786. &lt;p&gt; If the total network-wide stake exceeds this value, the effective staking reward ratio MUST be reduced to maintain solvency of the staking reward account. |
| max_total_reward | [int64](#int64) |  | A limit amount that could be paid as staking rewards.&lt;br/&gt; In the limit case, the network could pay at most this amount, in tinybar as staking rewards for the staking period that just ended, if all other conditions were met to perfection.&lt;br/&gt; &lt;p&gt; This value SHALL reflect the result of a maximum reward calculation that takes into account the balance thresholds and maximum stake thresholds as defined in HIP-782 and HIP-786.&lt;br/&gt; This value is a convenience. The actual defined calculation SHALL be authoritative in the unlikely event this value differs. |





 

 

 

 



## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |

