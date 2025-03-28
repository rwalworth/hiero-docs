# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/token/staking_node_info.proto](#services_state_token_staking_node_info-proto)
    - [StakingNodeInfo](#proto-StakingNodeInfo)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_token_staking_node_info-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/token/staking_node_info.proto
# Staking Node Information
This is an entry describing the staking characteristics of a single active
node.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-StakingNodeInfo"></a>

### StakingNodeInfo
An Hedera Token Service staking info entity.

Staking info is per node. Shard and Realm are implied based on the network
address book entry for this node.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_number | [int64](#int64) |  | The entity number of this node.&lt;br/&gt; The shard and realm numbers are implied, based on the network address book entry for this node. &lt;p&gt; This value SHALL be unique within a given shard and realm.&lt;br/&gt; This value MAY be repeated across shards and/or realms. |
| min_stake | [int64](#int64) |  | The minimum stake, in tinybar, that is required for this node to have a non-zero weight in the network consensus. &lt;p&gt; If the current value of `stake` is below this value, this node SHALL have a zero weight in network consensus. |
| max_stake | [int64](#int64) |  | The maximum stake to this node that is considered to calculate its weight in the network consensus. &lt;p&gt; If the current `stake` value is above this limit, the excess staked HBAR SHALL NOT be considered when determining consensus weight. |
| stake_to_reward | [int64](#int64) |  | The sum of balances of all accounts staked to this node, considering only accounts that choose to receive rewards. |
| stake_to_not_reward | [int64](#int64) |  | The sum of balances of all accounts staked to this node, considering only accounts that decline to receive rewards. |
| stake_reward_start | [int64](#int64) |  | The snapshot of stake_to_reward value at the beginning of the current staking period. &lt;p&gt; This is necessary when calculating rewards for the current staking period without considering changes to `stake_to_reward` _within_ the current staking period.&lt;br/&gt; This value SHALL be reset at the beginning of every staking period. |
| unclaimed_stake_reward_start | [int64](#int64) |  | The amount of staked HBAR from `stake_reward_start` that will have unclaimed rewards due to accounts changing their staking metadata in a way that disqualifies them for the current staking period. &lt;p&gt; This value SHALL be reset at the beginning of every staking period. |
| stake | [int64](#int64) |  | The total amount of HBAR staked to this node. &lt;p&gt; This is sum of stake_to_reward and stake_to_not_reward.&lt;br/&gt; If the sum is greater than `max_stake`, then the _effective_ stake SHALL be `max_stake`.&lt;br/&gt; If the sum is less than `min_stake`, then the _effective_ stake SHALL be `0`. |
| reward_sum_history | [int64](#int64) | repeated | A running list of reward amounts for the last 365&#43;1 staking periods (typically a year and a day). &lt;p&gt; The first element SHALL be the reward up to and including the last full period prior to the present reward period.&lt;br/&gt; The second element SHALL be the reward up to and including the period before the last full period prior to the present period.&lt;br/&gt; The list SHALL continue in reverse chronological order until the reward history limit is reached. |
| weight | [int32](#int32) |  | **Deprecated.** The consensus weight of this node in the network. &lt;p&gt; This is recomputed based on the `stake` of this node at midnight UTC of each day. If the `stake` of this node at that time is less than `min_stake`, then the weight SHALL be 0.&lt;br/&gt; &lt;p&gt; Given the following: &lt;ul&gt; &lt;li&gt;The `effective stake` of a single node SHALL be `0` if the node `stake` is less than `min_stake`.&lt;/li&gt; &lt;li&gt;The `effective stake` SHALL be `max_stake` if the node `stake` is greater than `max_stake`.&lt;/li&gt; &lt;li&gt;The `effective stake` SHALL be the actual value of `stake` if `min_stake` &lt; `stake` &lt; `max_stake`.&lt;/li&gt; &lt;li&gt;The `effective network stake` SHALL be calculated as ∑(`effective stake` of each node) for all nodes in the network address book.&lt;/li&gt; &lt;/ul&gt; &lt;p&gt; This field is deprecated and SHALL NOT be used when RosterLifecycle is enabled. The weight SHALL be same as the `effective_stake` described above. |
| pending_rewards | [int64](#int64) |  | The total staking rewards in tinybars that MAY be collected by all accounts staking to the current node after the end of this staking period. &lt;p&gt; This SHALL be calculated assuming that no account &#34;renounces&#34; its rewards by setting `decline_reward` to true, or is ineligible for some other reason.&lt;br/&gt; When the current node is deleted, this amount SHALL be subtracted from the total pending rewards of all accounts staking to all nodes in the network in NetworkStakingRewards. |
| deleted | [bool](#bool) |  | A flag indicating that this node has been removed from this network. |





 

 

 

 



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

