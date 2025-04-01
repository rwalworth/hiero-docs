# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_create.proto](#services_token_create-proto)
    - [TokenCreateTransactionBody](#proto-TokenCreateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_create-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_create.proto
# Token Create
Create an Hedera Token Service (HTS) token.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenCreateTransactionBody"></a>

### TokenCreateTransactionBody
Create an HTS token.

#### Keys
Each token has several keys that, separately, control different functions
for that token. It is *_strongly_* recommended that each key assigned to
a token be unique, or disabled by assigning an empty `KeyList`.
Keys and purpose
- `adminKey` is a general access and may authorize a token update
  transaction as well as _update the other keys_. Even the admin key
  cannot authorize _adding_ a key that is not present, however.&lt;br/&gt;
  The admin key may also delete the token entirely.
- `fee_schedule` may authorize updating the token custom fees. If this
  key is not present, the custom fees for the token are fixed and immutable.
- `freeze` may authorize a token freeze or unfreeze transaction.
  If this key is not present, accounts holding this token cannot have
  their tokens frozen or unfrozen.
- `kyc` may authorize a token grant KYC or revoke KYC transaction.
  If this key is not present, accounts holding this token cannot have
  KYC status granted or revoked.
- `metadata` may authorize token update nfts transactions.
  If this key is not present, the token metadata values for that
  non-fungible/unique token _type_ will be immutable.
- `pause` may authorize a token pause or token unpause transaction.
  If this key is not present, the token cannot be paused (preventing any
  account from transacting in that token) or resumed.
- `supply` may authorize a token mint or burn transaction.
  If this key is not present, the token cannot mint additional supply and
  existing tokens cannot be &#34;burned&#34; from the treasury (but _might_ still be
  &#34;burned&#34; from individual accounts, c.f. `wipeKey` and `tokenWipe`).
- `wipe` may authorize a token wipe account transaction.
  If this key is not present, accounts holding this token cannot have
  their balance or NFTs wiped (effectively burned).

#### Requirements
If `tokenType` is fungible/common, the `initialSupply` MUST be strictly
greater than zero(`0`).&lt;br/&gt;
If `tokenType` is non-fungible/unique, the `initialSupply` MUST
be zero(`0`).&lt;br/&gt;
If `tokenSupplyType` is &#34;infinite&#34;, the `maxSupply` MUST be zero(`0`).&lt;br/&gt;
If `tokenSupplyType` is &#34;finite&#34;, the `maxSupply` MUST be strictly
greater than zero(`0`).&lt;br/&gt;

### Block Stream Effects
If the token is created, the Token Identifier SHALL be in the receipt.&lt;br/&gt;


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | A name for the token.&lt;br/&gt; This is generally the &#34;full name&#34; displayed in wallet software. &lt;p&gt; This field is REQUIRED.&lt;br/&gt; This value MUST NOT exceed 100 bytes when encoded as UTF-8.&lt;br/&gt; This value MUST NOT contain the Unicode NUL codepoint. |
| symbol | [string](#string) |  | A symbol to use for the token. &lt;p&gt; This field is REQUIRED.&lt;br/&gt; This value MUST NOT exceed 100 bytes when encoded as UTF-8.&lt;br/&gt; This value MUST NOT contain the Unicode NUL codepoint. |
| decimals | [uint32](#uint32) |  | A decimal precision of the token&#39;s smallest denomination.&lt;br/&gt; Most values are described in terms of this smallest denomination, so the token initial supply, for instance, must be divided by &lt;tt&gt;10&lt;sup&gt;decimals&lt;/sup&gt;&lt;/tt&gt; to get whole tokens. &lt;p&gt; This MUST be zero(`0`) for non-fungible/unique tokens. |
| initialSupply | [uint64](#uint64) |  | An initial supply, in the smallest denomination for the token. &lt;p&gt; This amount SHALL be transferred to the treasury account as part of this transaction.&lt;br/&gt; This amount MUST be specified in the smallest denomination for the token (i.e. &lt;tt&gt;10&lt;sup&gt;-decimals&lt;/sup&gt;&lt;/tt&gt; whole tokens).&lt;br/&gt; This MUST be zero(`0`) for a non-fungible/unique token. |
| treasury | [AccountID](#proto-AccountID) |  | A treasury account identifier. &lt;p&gt; This field is REQUIRED.&lt;br/&gt; The identified account SHALL be designated the &#34;treasury&#34; for the new token, and all tokens &#34;minted&#34; SHALL be delivered to that account, including the initial supply, if any.&lt;br/&gt; The identified account MUST exist, MUST NOT be expired, and SHOULD have a non-zero HBAR balance.&lt;br/&gt; The identified account SHALL be associated to the new token. |
| adminKey | [Key](#proto-Key) |  | An Hedera key for token administration. &lt;p&gt; This key, if set, SHALL have administrative authority for this token and MAY authorize token update and/or token delete transactions.&lt;br/&gt; If this key is not set, or is an empty `KeyList`, this token SHALL be immutable, except for expiration and renewal. |
| kycKey | [Key](#proto-Key) |  | An Hedera key for managing account KYC. &lt;p&gt; This key, if set, SHALL have KYC authority for this token and MAY authorize transactions to grant or revoke KYC for accounts.&lt;br/&gt; If this key is not set, or is an empty `KeyList`, KYC status for this token SHALL NOT be granted or revoked for any account.&lt;br/&gt; If this key is removed after granting KYC, those grants SHALL remain and cannot be revoked. |
| freezeKey | [Key](#proto-Key) |  | An Hedera key for managing asset &#34;freeze&#34;. &lt;p&gt; This key, if set, SHALL have &#34;freeze&#34; authority for this token and MAY authorize transactions to freeze or unfreeze accounts with respect to this token.&lt;br/&gt; If this key is not set, or is an empty `KeyList`, this token SHALL NOT be frozen or unfrozen for any account.&lt;br/&gt; If this key is removed after freezing accounts, those accounts SHALL remain frozen and cannot be unfrozen. |
| wipeKey | [Key](#proto-Key) |  | An Hedera key for wiping tokens from accounts. &lt;p&gt; This key, if set, SHALL have &#34;wipe&#34; authority for this token and MAY authorize transactions to &#34;wipe&#34; any amount of this token from any account, effectively burning the tokens &#34;wiped&#34;.&lt;br/&gt; If this key is not set, or is an empty `KeyList`, it SHALL NOT be possible to &#34;wipe&#34; this token from an account. |
| supplyKey | [Key](#proto-Key) |  | An Hedera key for &#34;minting&#34; and &#34;burning&#34; tokens. &lt;p&gt; This key, if set, MAY authorize transactions to &#34;mint&#34; new tokens to be delivered to the token treasury or &#34;burn&#34; tokens held by the token treasury.&lt;br/&gt; If this key is not set, or is an empty `KeyList`, it SHALL NOT be possible to change the supply of tokens and neither &#34;mint&#34; nor &#34;burn&#34; transactions SHALL be permitted. |
| freezeDefault | [bool](#bool) |  | An initial Freeze status for accounts associated to this token. &lt;p&gt; If this value is set, an account MUST be the subject of a `tokenUnfreeze` transaction after associating to the token before that account can send or receive this token.&lt;br/&gt; If this value is set, the `freezeKey` SHOULD be set.&lt;br/&gt; If the `freezeKey` is not set, any account associated to this token while this value is set SHALL be permanently frozen. &lt;p&gt; &lt;blockquote&gt;REVIEW NOTE&lt;blockquote&gt; Should we prevent setting this value true for tokens with no freeze key?&lt;br/&gt; Should we set this value to false if a freeze key is removed? &lt;/blockquote&gt;&lt;/blockquote&gt; |
| expiry | [Timestamp](#proto-Timestamp) |  | An expiration timestamp. &lt;p&gt; If the `autoRenewAccount` and `autoRenewPeriod` fields are set, this value SHALL be replaced with the current consensus time extended by the `autoRenewPeriod` duration.&lt;br/&gt; If this value is set and token expiration is enabled in network configuration, this token SHALL expire when consensus time exceeds this value, and MAY be subsequently removed from the network state.&lt;br/&gt; If this value is not set, and the automatic renewal account is also not set, then this value SHALL default to the current consensus time extended by the &#34;default&#34; expiration period from network configuration. |
| autoRenewAccount | [AccountID](#proto-AccountID) |  | An identifier for the account to be charged renewal fees at the token&#39;s expiry to extend the lifetime of the token. &lt;p&gt; If this value is set, the token lifetime SHALL be extended by the _smallest_ of the following: &lt;ul&gt; &lt;li&gt;The current `autoRenewPeriod` duration.&lt;/li&gt; &lt;li&gt;The maximum duration that this account has funds to purchase.&lt;/li&gt; &lt;li&gt;The configured MAX_AUTORENEW_PERIOD at the time of automatic renewal.&lt;/li&gt; &lt;/ul&gt; If this account&#39;s HBAR balance is `0` when the token must be renewed, then the token SHALL be expired, and MAY be subsequently removed from state.&lt;br/&gt; If this value is set, the referenced account MUST sign this transaction. |
| autoRenewPeriod | [Duration](#proto-Duration) |  | A duration between token automatic renewals.&lt;br/&gt; All entities in state may be charged &#34;rent&#34; occasionally (typically every 90 days) to prevent unnecessary growth of the ledger. This value sets the interval between such events for this token. &lt;p&gt; This value MUST be set.&lt;br/&gt; This value MUST be greater than the configured MIN_AUTORENEW_PERIOD.&lt;br/&gt; This value MUST be less than the configured MAX_AUTORENEW_PERIOD. |
| memo | [string](#string) |  | A short description for this token. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| tokenType | [TokenType](#proto-TokenType) |  | A type for this token, according to IWA classification. &lt;p&gt; If this value is not set, the token SHALL have the default type of fungible/common.&lt;br/&gt; This field SHALL be immutable. |
| supplyType | [TokenSupplyType](#proto-TokenSupplyType) |  | A supply type for this token, according to IWA classification. &lt;p&gt; If this value is not set, the token SHALL have the default supply type of &#34;infinite&#34; (which is, as a practical matter, (2&lt;sup&gt;&lt;i&gt;63&lt;/i&gt;&lt;/sup&gt;-1)/10&lt;sup&gt;&lt;i&gt;decimals&lt;/i&gt;&lt;/sup&gt;).&lt;br/&gt; This field SHALL be immutable. |
| maxSupply | [int64](#int64) |  | A maximum supply for this token. &lt;p&gt; This SHALL be interpreted in terms of the smallest fractional unit for this token.&lt;br/&gt; If `supplyType` is &#34;infinite&#34;, this MUST be `0`.&lt;br/&gt; This field SHALL be immutable. |
| fee_schedule_key | [Key](#proto-Key) |  | An Hedera key for managing the token custom fee schedule. &lt;p&gt; This key, if set, MAY authorize transactions to modify the `custom_fees` for this token.&lt;br/&gt; If this key is not set, or is an empty `KeyList`, the `custom_fees` for this token SHALL NOT be modified. |
| custom_fees | [CustomFee](#proto-CustomFee) | repeated | A list of custom fees representing a fee schedule. &lt;p&gt; This list MAY be empty, which SHALL mean that there are no custom fees for this token.&lt;br/&gt; If this token is a non-fungible/unique type, the entries in this list MUST NOT declare a `fractional_fee`.&lt;br/&gt; If this token is a fungible/common type, the entries in this list MUST NOT declare a `royalty_fee`.&lt;br/&gt; Any token type MAY include entries that declare a `fixed_fee`. |
| pause_key | [Key](#proto-Key) |  | An Hedera key for managing token &#34;pause&#34;. &lt;p&gt; This key, if set, SHALL have &#34;pause&#34; authority for this token and MAY authorize transactions to pause or unpause this token.&lt;br/&gt; If this key is not set, or is an empty `KeyList`, this token SHALL NOT be paused or unpaused.&lt;br/&gt; If this key is removed while the token is paused, the token cannot be unpaused and SHALL remain paused. |
| metadata | [bytes](#bytes) |  | Token &#34;Metadata&#34;. &lt;p&gt; The value, if set, MUST NOT exceed 100 bytes.&lt;br/&gt; &lt;dl&gt;&lt;dt&gt;Examples&lt;/dt&gt; &lt;dd&gt;hcs://1/0.0.4896575&lt;/dd&gt; &lt;dd&gt;ipfs://bafkreifd7tcjjuwxxf4qkaibkj62pj4mhfuud7plwrc3pfoygt55al6syi&lt;/dd&gt; &lt;/dl&gt; |
| metadata_key | [Key](#proto-Key) |  | An Hedera key for managing the token `metadata`. &lt;p&gt; This key, if set, MAY authorize transactions to modify the `metadata` for this token.&lt;br/&gt; If this key is not set, or is an empty `KeyList`, the `metadata` for this token SHALL NOT be modified. |





 

 

 

 



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

