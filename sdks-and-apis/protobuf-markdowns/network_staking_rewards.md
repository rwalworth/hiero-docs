# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/token/network_staking_rewards.proto](#services_state_token_network_staking_rewards-proto)
    - [NetworkStakingRewards](#proto-NetworkStakingRewards)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_token_network_staking_rewards-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/token/network_staking_rewards.proto
# Network Staking Rewards
The information needed to calculate the staking rewards for all nodes in
the network.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-NetworkStakingRewards"></a>

### NetworkStakingRewards
An Hedera Token Service staking reward entity.
This stores values related to the aggregate staking rewards for all nodes in
the network. It is calculated at the beginning of each staking period.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| staking_rewards_activated | [bool](#bool) |  | A flag indicating that staking rewards are activated on the network. &lt;p&gt; Among other criteria, this is set to true when the balance of 0.0.800 (the account that pays rewards) reaches a minimum required balance. |
| total_staked_reward_start | [int64](#int64) |  | A global snapshot of the `stake_to_reward` value for all nodes at the beginning of the current staking period. &lt;p&gt; The `stake_to_reward` value is the sum of balance and `staked_to_me` for all accounts staked to a node that do not decline staking rewards.&lt;br/&gt; This is needed for calculating rewards for current staking period without considering changes to `stake_to_reward` within the current staking period.&lt;br/&gt; This value SHALL be reset at the beginning of every staking period. |
| total_staked_start | [int64](#int64) |  | A global snapshot of the `stake` value for all nodes at the beginning of the current staking period. &lt;p&gt; The `stake` value is the sum of balance and `staked_to_me` for all accounts staked to a node, and SHALL NOT consider whether the account has accepted or declined rewards.&lt;br/&gt; This value SHALL be reset at the beginning of every staking period. |
| pending_rewards | [int64](#int64) |  | The total staking rewards, in tinybars, that may be collected by all accounts staking to all nodes after the end of this staking period. &lt;p&gt; This SHALL be calculated assuming that no account &#34;renounces&#34; its rewards by setting `decline_reward` to true, or is ineligible for some other reason.&lt;br/&gt; If a node is removed, the `pending_rewards` value of that node SHALL be subtracted from this value. |





 

 

 

 



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

