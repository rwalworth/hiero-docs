# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/contract_update.proto](#services_contract_update-proto)
    - [ContractUpdateTransactionBody](#proto-ContractUpdateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_contract_update-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/contract_update.proto
# Contract Update
Modify a smart contract. Any change other than updating the expiration time
requires that the contract be modifiable (has a valid `adminKey`) and that
the transaction be signed by the `adminKey`

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ContractUpdateTransactionBody"></a>

### ContractUpdateTransactionBody
Modify the current state of a smart contract.

### Requirements
- The `adminKey` MUST sign all contract update transactions except one
  that only updates the `expirationTime`.
- A transaction that modifies any field other than `expirationTime` for
  a contract without a valid `adminKey` set SHALL fail with response
  code `MODIFYING_IMMUTABLE_CONTRACT`.
- Fields set to non-default values in this transaction SHALL be updated on
  success. Fields not set to non-default values SHALL NOT be
  updated on success.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractID | [ContractID](#proto-ContractID) |  | The contact ID that identifies the smart contract to be updated.&lt;br/&gt; This field MUST be set, and MUST NOT be a default ID (`0.0.0`). |
| expirationTime | [Timestamp](#proto-Timestamp) |  | If set, modify the time at which this contract will expire.&lt;br/&gt; An expired contract requires a rent payment to &#34;renew&#34; the contract. A transaction to update this field is how that rent payment is made. &lt;p&gt; This value MUST NOT be less than the current `expirationTime` of the contract. If this value is earlier than the current value, the transaction SHALL fail with response code `EXPIRATION_REDUCTION_NOT_ALLOWED`. |
| adminKey | [Key](#proto-Key) |  | If set, modify the key that authorizes updates to the contract. &lt;p&gt; If this field is set to a valid Key, this key and the previously set key MUST both sign this transaction.&lt;br/&gt; If this value is an empty `KeyList`, the prior key MUST sign this transaction, and the smart contract SHALL be immutable after this transaction completes, except for expiration and renewal.&lt;br/&gt; If this value is not an empty `KeyList`, but does not contain any cryptographic keys, or is otherwise malformed, this transaction SHALL fail with response code `INVALID_ADMIN_KEY`. |
| proxyAccountID | [AccountID](#proto-AccountID) |  | **Deprecated.** Replaced with `staked_id` alternatives. This field is unused and SHALL NOT modify the contract state.&lt;br/&gt; The id of an account to which the contract is proxy staked |
| autoRenewPeriod | [Duration](#proto-Duration) |  | If set, modify the duration added to expiration time by each auto-renewal to this value. |
| fileID | [FileID](#proto-FileID) |  | **Deprecated.** This field is unused and SHALL NOT modify the contract state.&lt;br/&gt; Previously, an ID of a file containing the bytecode of the Solidity transaction that created this contract. |
| memo | [string](#string) |  | **Deprecated.** This value could not accurately distinguish unset or deliberately empty. memoWrapper should be used instead.&lt;br/&gt; |
| memoWrapper | [google.protobuf.StringValue](#google-protobuf-StringValue) |  | If set, modify the short memo for this smart contract. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| max_automatic_token_associations | [google.protobuf.Int32Value](#google-protobuf-Int32Value) |  | If set, modify the maximum number of tokens that can be auto-associated with the contract. &lt;p&gt; If this is set and less than or equal to `used_auto_associations`, or 0, then this contract MUST manually associate with a token before transacting in that token.&lt;br/&gt; This value MAY also be `-1` to indicate no limit.&lt;br/&gt; This value MUST NOT be less than `-1`. |
| auto_renew_account_id | [AccountID](#proto-AccountID) |  | If set, modify the account, in the same shard and realm as this smart contract, that has agreed to allow the network to use its balance, when needed, to automatically extend this contract&#39;s expiration time. &lt;p&gt; If this field is set to a non-default value, that Account MUST sign this transaction.&lt;br/&gt; If this field is set to a default AccountID value (`0.0.0`), any pre-existing `auto_renew_account_id` value SHALL be removed on success. |
| staked_account_id | [AccountID](#proto-AccountID) |  | An account identifier.&lt;br/&gt; A staked account acts as a proxy, and this contract effectively nominates the same node as the identified account. &lt;p&gt; If set, modify this smart contract such that it SHALL stake its HBAR to the same node as the identified account.&lt;br/&gt; If this field is set to a default AccountID value (`0.0.0`), any pre-existing `staked_account_id` value SHALL be removed on success. |
| staked_node_id | [int64](#int64) |  | A node identifier.&lt;br/&gt; A staked node identifier indicates the consensus node that this account nominates for staking. &lt;p&gt; If set, modify this smart contract such that it SHALL stake its HBAR to this node. If set to a the value `-1` any pre-existing `staked_node_id` value SHALL be removed on success. &lt;p&gt; &lt;blockquote&gt;Note: node IDs do fluctuate as node operators change. Most contracts are immutable, and a contract staking to an invalid node ID SHALL NOT participate in staking. Immutable contracts may find it more reliable to use a proxy account for staking (via `staked_account_id`) to enable updating the _effective_ staking node ID when necessary through updating the proxy account.&lt;/blockquote&gt; |
| decline_reward | [google.protobuf.BoolValue](#google-protobuf-BoolValue) |  | A flag indicating if staking rewards are declined.&lt;br/&gt; If set, modify the flag indicating if this contract declines to accept rewards for staking its HBAR to secure the network. &lt;p&gt; If set to true, this smart contract SHALL NOT receive any reward for staking its HBAR balance to help secure the network, regardless of staking configuration, but MAY stake HBAR to support the network without reward. |





 

 

 

 



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

