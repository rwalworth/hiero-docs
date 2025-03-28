# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/crypto_create.proto](#services_crypto_create-proto)
    - [CryptoCreateTransactionBody](#proto-CryptoCreateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_crypto_create-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/crypto_create.proto
# Crypto Create
Messages to create a new end-user account within the distributed ledger.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-CryptoCreateTransactionBody"></a>

### CryptoCreateTransactionBody
Create a new account.

If the auto_renew_account field is set, the key of the referenced account
MUST sign this transaction.&lt;br/&gt;
Current limitations REQUIRE that `shardID` and `realmID` both MUST be `0`.
This is expected to change in the future.

### Block Stream Effects
The newly created account SHALL be included in State Changes.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [Key](#proto-Key) |  | The identifying key for this account. This key represents the account owner, and is required for most actions involving this account that do not modify the account itself. This key may also identify the account for smart contracts. &lt;p&gt; This field is REQUIRED. This `Key` MUST NOT be an empty `KeyList` and MUST contain at least one &#34;primitive&#34; (i.e. cryptographic) key value. |
| initialBalance | [uint64](#uint64) |  | An amount, in tinybar, to deposit to the newly created account. &lt;p&gt; The deposited amount SHALL be debited to the &#34;payer&#34; account for this transaction. |
| proxyAccountID | [AccountID](#proto-AccountID) |  | **Deprecated.** Use `staked_id` instead.&lt;br/&gt; An account identifier for a staking proxy. |
| sendRecordThreshold | [uint64](#uint64) |  | **Deprecated.** Removed prior to the first available history, and may be related to an early design dead-end.&lt;br/&gt; An amount below which record stream records would not be created for a transaction that reduces this account balance. |
| receiveRecordThreshold | [uint64](#uint64) |  | **Deprecated.** Removed prior to the first available history, and may be related to an early design dead-end.&lt;br/&gt; An amount below which record stream records would not be created for a transaction that increases this account balance. |
| receiverSigRequired | [bool](#bool) |  | A flag indicating the account holder must authorize all incoming token transfers. &lt;p&gt; If this flag is set then any transaction that would result in adding hbar or other tokens to this account balance MUST be signed by the identifying key of this account (that is, the `key` field).&lt;br/&gt; If this flag is set, then the account key (`key` field) MUST sign this create transaction, in addition to the transaction payer. |
| autoRenewPeriod | [Duration](#proto-Duration) |  | The duration between account automatic renewals.&lt;br/&gt; All entities in state may be charged &#34;rent&#34; occasionally (typically every 90 days) to prevent unnecessary growth of the ledger. This value sets the interval between such events for this account. &lt;p&gt; If the account balance (in HBAR) is insufficient to pay the full renewal fee, the entire HBAR balance SHALL be consumed and the expiration for the account SHALL be extended as far as the available balance can support.&lt;br/&gt; If the account HBAR balance is `0` when the account must be renewed, then the account SHALL be deleted, and subsequently removed from state. |
| shardID | [ShardID](#proto-ShardID) |  | The shard in which this account is created &lt;p&gt; Currently, this MUST be `0`.&lt;br/&gt; If the desired shard is `0`, this SHOULD NOT be set. |
| realmID | [RealmID](#proto-RealmID) |  | The realm in which this account is created. &lt;p&gt; The shard number for this realm MUST match the value in `shardID`.&lt;br/&gt; Currently, this MUST be `0` for both fields.&lt;br/&gt; If the desired realm is `0`, this SHOULD NOT be set. |
| newRealmAdminKey | [Key](#proto-Key) |  | **Deprecated.** This field was never actually used or enabled, and is not expected to ever be used in the future.&lt;br/&gt; |
| memo | [string](#string) |  | A short description of this Account. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| max_automatic_token_associations | [int32](#int32) |  | A maximum number of tokens that can be auto-associated with this account.&lt;br/&gt; By default this value is 0 for all accounts except for automatically created accounts (e.g. smart contracts), which default to -1. &lt;p&gt; If this value is `0`, then this account MUST manually associate to a token before holding or transacting in that token.&lt;br/&gt; This value MAY also be `-1` to indicate no limit.&lt;br/&gt; This value MUST NOT be less than `-1`. |
| staked_account_id | [AccountID](#proto-AccountID) |  | ID of the account to which this account is staking its balances. &lt;p&gt; If this account is not currently staking its balances, then this field, if set, MUST be the sentinel value of `0.0.0`. |
| staked_node_id | [int64](#int64) |  | ID of the node this account is staked to. &lt;p&gt; If this account is not currently staking its balances, then this field, if set, SHALL be the sentinel value of `-1`.&lt;br/&gt; Wallet software SHOULD surface staking issues to users and provide a simple mechanism to update staking to a new node ID in the event the prior staked node ID ceases to be valid. |
| decline_reward | [bool](#bool) |  | A boolean indicating that this account has chosen to decline rewards for staking its balances. &lt;p&gt; This account MAY still stake its balances, but SHALL NOT receive reward payments for doing so, if this value is set. |
| alias | [bytes](#bytes) |  | Bytes to be used as the account&#39;s alias. &lt;p&gt; This value, if set, MUST be one of the following values&lt;br/&gt; &lt;ul&gt; &lt;li&gt;The 32-byte serialized form of the ED25519 account key.&lt;/li&gt; &lt;li&gt;The 33-byte _compressed_ serialized form of the ECDSA(secp256k1) account key.&lt;/li&gt; &lt;li&gt;The 20-byte EVM address derived from a keccak-256 hash of the ECDSA(secp256k1) account key&lt;/li&gt; &lt;/ul&gt; All aliases within the network MUST be unique. If this value matches an existing account alias, this `create` transaction SHALL fail.&lt;br/&gt; If an account exists with a particular alias value, any transaction to transfer value _to_ that alias SHALL deposit the transferred value in the existing account, and SHALL NOT assess an account creation fee.&lt;br/&gt; Once set, an account alias is immutable and MUST NOT be changed. |





 

 

 

 



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

