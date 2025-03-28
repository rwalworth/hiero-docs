# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/crypto_update.proto](#services_crypto_update-proto)
    - [CryptoUpdateTransactionBody](#proto-CryptoUpdateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_crypto_update-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/crypto_update.proto
# Crypto Update
Modify a single account.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-CryptoUpdateTransactionBody"></a>

### CryptoUpdateTransactionBody
Modify the current state of an account.

### Requirements
- The `key` for this account MUST sign all account update transactions.
- If the `key` field is set for this transaction, then _both_ the current
  `key` and the new `key` MUST sign this transaction, for security and to
  prevent setting the `key` field to an invalid value.
- If the `auto_renew_account` field is set for this transaction, the account
  identified in that field MUST sign this transaction.
- Fields set to non-default values in this transaction SHALL be updated on
  success. Fields not set to non-default values SHALL NOT be
  updated on success.
- All fields that may be modified in this transaction SHALL have a
  default value of unset (a.k.a. `null`).

### Block Stream Effects
None

NOTE: Seven deprecated fields should probably be removed and the
      field names reserved.
reserved 4,5,6,7,10,11,12
Also, the `receiverSigRequiredField` oneOf should be removed
around `receiverSigRequiredWrapper` and the field renamed
(both actions are &#34;safe&#34; in protobuf) to `receiver_signature_required`.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accountIDToUpdate | [AccountID](#proto-AccountID) |  | An account identifier.&lt;br/&gt; This identifies the account which is to be modified in this transaction. &lt;p&gt; This field is REQUIRED. |
| key | [Key](#proto-Key) |  | An account key.&lt;br/&gt; This may be a &#34;primitive&#34; key (a singly cryptographic key), or a composite key. &lt;p&gt; If set, this key MUST be a valid key.&lt;br/&gt; If set, the previous key and new key MUST both sign this transaction. |
| proxyAccountID | [AccountID](#proto-AccountID) |  | **Deprecated.** Removed in favor of the `staked_id` oneOf.&lt;br/&gt; An account identifier for a &#34;proxy&#34; account. This account&#39;s HBAR are staked to a node selected by the proxy account. |
| proxyFraction | [int32](#int32) |  | **Deprecated.** Removed prior to the first available history.&lt;br/&gt; A fraction to split staking rewards between this account and the proxy account. |
| sendRecordThreshold | [uint64](#uint64) |  | **Deprecated.** Removed prior to the first available history, and may be related to an early design dead-end.&lt;br/&gt; The new threshold amount (in tinybars) for which an account record is created for any send/withdraw transaction |
| sendRecordThresholdWrapper | [google.protobuf.UInt64Value](#google-protobuf-UInt64Value) |  | **Deprecated.** Removed prior to the first available history, and may be related to an early design dead-end.&lt;br/&gt; The new threshold amount (in tinybars) for which an account record is created for any send/withdraw transaction |
| receiveRecordThreshold | [uint64](#uint64) |  | **Deprecated.** Removed prior to the first available history, and may be related to an early design dead-end.&lt;br/&gt; The new threshold amount (in tinybars) for which an account record is created for any receive/deposit transaction. |
| receiveRecordThresholdWrapper | [google.protobuf.UInt64Value](#google-protobuf-UInt64Value) |  | **Deprecated.** Removed prior to the first available history, and may be related to an early design dead-end.&lt;br/&gt; The new threshold amount (in tinybars) for which an account record is created for any receive/deposit transaction. |
| autoRenewPeriod | [Duration](#proto-Duration) |  | A duration to extend account expiration.&lt;br/&gt; An amount of time, in seconds, to extend the expiration date for this account when _automatically_ renewed. &lt;p&gt; This duration MUST be between the current configured minimum and maximum values defined for the network.&lt;br/&gt; This duration SHALL be applied only when _automatically_ extending the account expiration. |
| expirationTime | [Timestamp](#proto-Timestamp) |  | A new account expiration time, in seconds since the epoch. &lt;p&gt; For this purpose, `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`.&lt;br/&gt; If set, this value MUST be later than the current consensus time.&lt;br/&gt; If set, this value MUST be earlier than the current consensus time extended by the current maximum expiration time configured for the network. |
| receiverSigRequired | [bool](#bool) |  | **Deprecated.** Removed to distinguish between unset and a default value.&lt;br/&gt; Do NOT use this field to set a false value because the server cannot distinguish from the default value. Use receiverSigRequiredWrapper field for this purpose. |
| receiverSigRequiredWrapper | [google.protobuf.BoolValue](#google-protobuf-BoolValue) |  | A flag indicating the account holder must authorize all incoming token transfers. &lt;p&gt; If this flag is set then any transaction that would result in adding hbar or other tokens to this account balance MUST be signed by the identifying key of this account (that is, the `key` field). |
| memo | [google.protobuf.StringValue](#google-protobuf-StringValue) |  | A short description of this Account. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| max_automatic_token_associations | [google.protobuf.Int32Value](#google-protobuf-Int32Value) |  | A maximum number of tokens that can be auto-associated with this account.&lt;br/&gt; By default this value is 0 for all accounts except for automatically created accounts (i.e smart contracts) which default to -1. &lt;p&gt; If this value is `0`, then this account MUST manually associate to a token before holding or transacting in that token.&lt;br/&gt; This value MAY also be `-1` to indicate no limit.&lt;br/&gt; If set, this value MUST NOT be less than `-1`.&lt;br/&gt; |
| staked_account_id | [AccountID](#proto-AccountID) |  | ID of the account to which this account is staking its balances. &lt;p&gt; If this account is not currently staking its balances, then this field, if set, MUST be the sentinel value of `0.0.0`. |
| staked_node_id | [int64](#int64) |  | ID of the node this account is staked to. &lt;p&gt; If this account is not currently staking its balances, then this field, if set, SHALL be the sentinel value of `-1`.&lt;br/&gt; Wallet software SHOULD surface staking issues to users and provide a simple mechanism to update staking to a new node ID in the event the prior staked node ID ceases to be valid. |
| decline_reward | [google.protobuf.BoolValue](#google-protobuf-BoolValue) |  | A boolean indicating that this account has chosen to decline rewards for staking its balances. &lt;p&gt; This account MAY still stake its balances, but SHALL NOT receive reward payments for doing so, if this value is set, and `true`. |





 

 

 

 



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

