# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_update.proto](#services_token_update-proto)
    - [TokenUpdateTransactionBody](#proto-TokenUpdateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_update-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_update.proto
# Token Update
Modify the characteristics of an existing token. Most changes require that
the transaction be signed by an `admin_key`, and if that key is not valid
the only change permitted is to extend the token expiration.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenUpdateTransactionBody"></a>

### TokenUpdateTransactionBody
Update an existing token.

This transaction SHALL NOT update any field that is not set.&lt;br/&gt;
Most changes MUST be signed by the current `admin_key` of the token. If the
token does not currently have a valid `admin_key`, then this transaction
MUST NOT set any value other than `expiry` or a non-admin key.&lt;br/&gt;
If the `treasury` is set to a new account, the new account MUST sign this
transaction.&lt;br/&gt;
If the `treasury` is set to a new account for a _non-fungible/unique_ token,
The current treasury MUST NOT hold any tokens, or the network configuration
property `tokens.nfts.useTreasuryWildcards` MUST be set.

#### Requirements for Keys
Any of the key values may be changed, even without an admin key, but the
key to be changed MUST have an existing valid key assigned, and both the
current key and the new key MUST sign the transaction.&lt;br/&gt;
A key value MAY be set to an empty `KeyList`. In this case the existing
key MUST sign this transaction, but the new value is not a valid key, and the
update SHALL effectively remove the existing key.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token | [TokenID](#proto-TokenID) |  | A token identifier. &lt;p&gt; This SHALL identify the token type to delete.&lt;br/&gt; The identified token MUST exist, and MUST NOT be deleted.&lt;br/&gt; If any field other than `expiry` is set, the identified token MUST have a valid `admin_key`. |
| symbol | [string](#string) |  | A new symbol to use for the token. &lt;p&gt; This value, if set, MUST NOT exceed 100 bytes when encoded as UTF-8.&lt;br/&gt; This value, if set, MUST NOT contain the Unicode NUL codepoint. |
| name | [string](#string) |  | A new name for the token.&lt;br/&gt; This is generally the &#34;full name&#34; displayed in wallet software. &lt;p&gt; This value, if set, MUST NOT exceed 100 bytes when encoded as UTF-8.&lt;br/&gt; This value, if set, MUST NOT contain the Unicode NUL codepoint. |
| treasury | [AccountID](#proto-AccountID) |  | A new treasury account identifier. &lt;p&gt; If set, - The identified account SHALL be designated the &#34;treasury&#34; for the token, and all tokens &#34;minted&#34; SHALL be delivered to that account following this transaction.&lt;br/&gt; - The identified account MUST exist, MUST NOT be expired, MUST NOT be deleted, and SHOULD have a non-zero HBAR balance.&lt;br/&gt; - The identified account SHALL be associated to this token. - The full balance of this token held by the prior treasury account SHALL be transferred to the new treasury account, if the token type is fungible/common. - If the token type is non-fungible/unique, the previous treasury account MUST NOT hold any tokens of this type. - The new treasury account key MUST sign this transaction. |
| adminKey | [Key](#proto-Key) |  | An Hedera key for token administration. &lt;p&gt; This key, if set, SHALL have administrative authority for this token and MAY authorize token update and/or token delete transactions.&lt;br/&gt; If this key is set to an empty `KeyList`, this token SHALL be immutable thereafter, except for expiration and renewal.&lt;br/&gt; If set, this key MUST be a valid key or an empty `KeyList`.&lt;br/&gt; If set to a valid key, the previous key and new key MUST both sign this transaction. |
| kycKey | [Key](#proto-Key) |  | An Hedera key for managing account KYC. &lt;p&gt; This key, if set, SHALL have KYC authority for this token and MAY authorize transactions to grant or revoke KYC for accounts.&lt;br/&gt; If this key is not set, or is an empty `KeyList`, KYC status for this token SHALL NOT be granted or revoked for any account.&lt;br/&gt; If this key is removed after granting KYC, those grants SHALL remain and cannot be revoked.&lt;br/&gt; If set, this key MUST be a valid key or an empty `KeyList`.&lt;br/&gt; If set to a valid key, the previous key and new key MUST both sign this transaction. |
| freezeKey | [Key](#proto-Key) |  | An Hedera key for managing asset &#34;freeze&#34;. &lt;p&gt; This key, if set, SHALL have &#34;freeze&#34; authority for this token and MAY authorize transactions to freeze or unfreeze accounts with respect to this token.&lt;br/&gt; If this key is set to an empty `KeyList`, this token SHALL NOT be frozen or unfrozen for any account.&lt;br/&gt; If this key is removed after freezing accounts, those accounts SHALL remain frozen and cannot be unfrozen.&lt;br/&gt; If set, this key MUST be a valid key or an empty `KeyList`.&lt;br/&gt; If set to a valid key, the previous key and new key MUST both sign this transaction. |
| wipeKey | [Key](#proto-Key) |  | An Hedera key for wiping tokens from accounts. &lt;p&gt; This key, if set, SHALL have &#34;wipe&#34; authority for this token and MAY authorize transactions to &#34;wipe&#34; any amount of this token from any account, effectively burning the tokens &#34;wiped&#34;.&lt;br/&gt; If this key is set to an empty `KeyList`, it SHALL NOT be possible to &#34;wipe&#34; this token from an account.&lt;br/&gt; If set, this key MUST be a valid key or an empty `KeyList`.&lt;br/&gt; If set to a valid key, the previous key and new key MUST both sign this transaction. |
| supplyKey | [Key](#proto-Key) |  | An Hedera key for &#34;minting&#34; and &#34;burning&#34; tokens. &lt;p&gt; This key, if set, MAY authorize transactions to &#34;mint&#34; new tokens to be delivered to the token treasury or &#34;burn&#34; tokens held by the token treasury.&lt;br/&gt; If this key is set to an empty `KeyList`, it SHALL NOT be possible to change the supply of tokens and neither &#34;mint&#34; nor &#34;burn&#34; transactions SHALL be permitted.&lt;br/&gt; If set, this key MUST be a valid key or an empty `KeyList`.&lt;br/&gt; If set to a valid key, the previous key and new key MUST both sign this transaction. |
| autoRenewAccount | [AccountID](#proto-AccountID) |  | An identifier for the account to be charged renewal fees at the token&#39;s expiry to extend the lifetime of the token. &lt;p&gt; If this value is set for the identified token, the token lifetime SHALL be extended by the _smallest_ of the following at expiration: &lt;ul&gt; &lt;li&gt;The current `autoRenewPeriod` duration.&lt;/li&gt; &lt;li&gt;The maximum duration that this account has funds to purchase.&lt;/li&gt; &lt;li&gt;The configured MAX_AUTORENEW_PERIOD at the time of automatic renewal.&lt;/li&gt; &lt;/ul&gt; If this account&#39;s HBAR balance is `0` when the token must be renewed, then the token SHALL be expired, and MAY be subsequently removed from state.&lt;br/&gt; If this value is set, the referenced account MUST sign this transaction. &lt;p&gt; &lt;blockquote&gt;Note&lt;blockquote&gt; It is not currently possible to remove an automatic renewal account. Once set, it can only be replaced by a valid account. &lt;/blockquote&gt;&lt;/blockquote&gt; |
| autoRenewPeriod | [Duration](#proto-Duration) |  | A duration between token automatic renewals.&lt;br/&gt; All entities in state may be charged &#34;rent&#34; occasionally (typically every 90 days) to prevent unnecessary growth of the ledger. This value sets the interval between such events for this token. &lt;p&gt; If set, this value MUST be greater than the configured `MIN_AUTORENEW_PERIOD`.&lt;br/&gt; If set, this value MUST be less than the configured `MAX_AUTORENEW_PERIOD`. |
| expiry | [Timestamp](#proto-Timestamp) |  | An expiration timestamp. &lt;p&gt; If this value is set, the automatic renewal account is not set for the identified token, and token expiration is enabled in network configuration, this token SHALL expire when the consensus time exceeds this value, and MAY be subsequently removed from the network state.&lt;br/&gt; If `autoRenewAccount` is set or the `auto_renew_account_id` is set for the identified token, the token SHALL be subject to automatic renewal when the consensus time exceeds this value. |
| memo | [google.protobuf.StringValue](#google-protobuf-StringValue) |  | A short description for this token. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| fee_schedule_key | [Key](#proto-Key) |  | An Hedera key for managing the token custom fee schedule. &lt;p&gt; This key, if set, MAY authorize transactions to modify the `custom_fees` for this token.&lt;br/&gt; If this key is set to an empty `KeyList`, the `custom_fees` for this token SHALL NOT be modified.&lt;br/&gt; If set, this key MUST be a valid key or an empty `KeyList`.&lt;br/&gt; If set to a valid key, the previous key and new key MUST both sign this transaction. |
| pause_key | [Key](#proto-Key) |  | An Hedera key for managing token &#34;pause&#34;. &lt;p&gt; This key, if set, SHALL have &#34;pause&#34; authority for this token and MAY authorize transactions to pause or unpause this token.&lt;br/&gt; If this key is set to an empty `KeyList`, this token SHALL NOT be paused or unpaused.&lt;br/&gt; If this key is removed while the token is paused, the token cannot be unpaused and SHALL remain paused.&lt;br/&gt; If set, this key MUST be a valid key or an empty `KeyList`.&lt;br/&gt; If set to a valid key, the previous key and new key MUST both sign this transaction. |
| metadata | [google.protobuf.BytesValue](#google-protobuf-BytesValue) |  | Token &#34;Metadata&#34;. &lt;p&gt; The value, if set, MUST NOT exceed 100 bytes.&lt;br/&gt; &lt;dl&gt;&lt;dt&gt;Examples&lt;/dt&gt; &lt;dd&gt;hcs://1/0.0.4896575&lt;/dd&gt; &lt;dd&gt;ipfs://bafkreifd7tcjjuwxxf4qkaibkj62pj4mhfuud7plwrc3pfoygt55al6syi&lt;/dd&gt; &lt;/dl&gt; |
| metadata_key | [Key](#proto-Key) |  | An Hedera key for managing the token `metadata`. &lt;p&gt; This key, if set, MAY authorize transactions to modify the `metadata` for this token.&lt;br/&gt; If this key is set to an empty `KeyList`, the `metadata` for this token SHALL NOT be modified.&lt;br/&gt; If set, this key MUST be a valid key or an empty `KeyList`.&lt;br/&gt; If set to a valid key, the previous key and new key MUST both sign this transaction. |
| key_verification_mode | [TokenKeyValidation](#proto-TokenKeyValidation) |  | Set a key validation mode.&lt;br/&gt; Any key may be updated by a transaction signed by the token `admin_key`. Each role key may _also_ sign a transaction to update that key. If a role key signs an update to change that role key both old and new key must sign the transaction, _unless_ this field is set to `NO_VALIDATION`, in which case the _new_ key is not required to sign the transaction (the existing key is still required).&lt;br/&gt; The primary intent for this field is to allow a role key (e.g. a `pause_key`) holder to &#34;remove&#34; that key from the token by signing a transaction to set that role key to an empty `KeyList`. &lt;p&gt; If set to `FULL_VALIDATION`, either the `admin_key` or _both_ current and new key MUST sign this transaction to update a &#34;key&#34; field for the identified token.&lt;br/&gt; If set to `NO_VALIDATION`, either the `admin_key` or the current key MUST sign this transaction to update a &#34;key&#34; field for the identified token.&lt;br/&gt; This field SHALL be treated as `FULL_VALIDATION` if not set. |





 

 

 

 



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

