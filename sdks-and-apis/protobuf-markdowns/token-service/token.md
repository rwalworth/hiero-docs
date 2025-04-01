# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/token/token.proto](#services_state_token_token-proto)
    - [Token](#proto-Token)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_token_token-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/token/token.proto
## Token
Tokens represent both fungible and non-fungible units of exchange.
The `Token` here represents a token within the network state.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-Token"></a>

### Token
An Hedera Token Service(HTS) token.

A token SHALL represent a fungible or non-fungible unit of exchange.&lt;br/&gt;
The specified Treasury Account SHALL receive the initial supply of tokens and
SHALL determine distribution of all tokens once minted.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_id | [TokenID](#proto-TokenID) |  | A unique identifier for this token. |
| name | [string](#string) |  | A human-readable name for this token. &lt;p&gt; This value MAY NOT be unique.&lt;br/&gt; This value SHALL NOT exceed 100 bytes when encoded as UTF-8. |
| symbol | [string](#string) |  | A human-readable symbol for the token. &lt;p&gt; This value SHALL NOT be unique.&lt;br/&gt; This value SHALL NOT exceed 100 bytes when encoded as UTF-8. |
| decimals | [int32](#int32) |  | A number of decimal places for this token. &lt;p&gt; If decimals are 8 or 11, then the number of whole tokens can be at most billions or millions, respectively. More decimals allows for a more finely-divided token, but also limits the maximum total supply. &lt;p&gt; Examples &lt;ul&gt; &lt;li&gt;Bitcoin satoshis (21 million whole tokens with 8 decimals).&lt;/li&gt; &lt;li&gt;Hedera tinybar (50 billion whole tokens with 8 decimals).&lt;/li&gt; &lt;li&gt;Bitcoin milli-satoshis (21 million whole tokens with 11 decimals).&lt;/li&gt; &lt;li&gt;Theoretical limit is roughly 92.2 billion with 8 decimals, or 92.2 million with 11 decimals.&lt;/li&gt; &lt;/ul&gt; All token amounts in the network are stored as integer amounts, with each unit representing 10&lt;sup&gt;-decimals&lt;/sup&gt; whole tokens. &lt;p&gt; For tokens with `token_type` set to `NON_FUNGIBLE_UNIQUE` this MUST be 0. |
| total_supply | [int64](#int64) |  | A _current_ total supply of this token, expressed in the smallest unit of the token. &lt;p&gt; The number of _whole_ tokens this represents is (total_supply / 10&lt;sup&gt;decimals&lt;/sup&gt;). The value of total supply, MUST be within the positive range of a twos-compliment signed 64-bit integer. The `total_supply`, therefore MUST be between 1, and 9,223,372,036,854,775,807, inclusive. &lt;p&gt; This value SHALL be reduced when a `token_burn` or `token_wipe_account` operation is executed, and SHALL be increased when a `token_mint` operation is executed. |
| treasury_account_id | [AccountID](#proto-AccountID) |  | A treasury account identifier for this token. &lt;p&gt; When the token is created, the initial supply given in the token create transaction SHALL be minted and deposited in the treasury account.&lt;br/&gt; All token mint transactions for this token SHALL deposit the new minted tokens in the treasury account.&lt;br/&gt; All token burn transactions for this token SHALL remove the tokens to be burned from the treasury account. |
| admin_key | [Key](#proto-Key) |  | Access control for general modification of this token. &lt;p&gt; This key MUST sign any `token_update` transaction that changes any attribute of the token other than expiration_time. Other attributes of this token MAY be changed by transactions other than `token_update`, and MUST be signed by one of the other purpose-specific keys assigned to the token.&lt;br/&gt; This value can be set during token creation, and SHALL NOT be modified thereafter, unless the update transaction is signed by both the existing `admin_key` and the new `admin_key`.&lt;br/&gt; If the `admin_key` is not set for a token, that token SHALL be immutable. |
| kyc_key | [Key](#proto-Key) |  | Access control for KYC for this token. &lt;p&gt; Know Your Customer (KYC) status may be granted for an account by a token grant kyc transaction signed by this key.&lt;br/&gt; If this key is not set, then KYC status cannot be granted to an account for this token, and any `TokenGrantKyc` transaction attempting to grant kyc to an account for this token SHALL NOT succeed.&lt;br/&gt; This key MAY be set when the token is created, and MAY be set or modified via a token update transaction signed by the `admin_key`.&lt;br/&gt; If `admin_key` is not set, this value, whether set or unset, SHALL be immutable. |
| freeze_key | [Key](#proto-Key) |  | Access control to freeze this token. &lt;p&gt; A token may be frozen for an account, preventing any transaction from transferring that token for that specified account, by a token freeze account transaction signed by this key.&lt;br/&gt; If this key is not set, the token cannot be frozen, and any transaction attempting to freeze the token for an account SHALL NOT succeed.&lt;br/&gt; This key MAY be set when the token is created, and MAY be set or modified via a token update transaction signed by the `admin_key`.&lt;br/&gt; If `admin_key` is not set, this value, whether set or unset, SHALL be immutable. |
| wipe_key | [Key](#proto-Key) |  | Access control of account wipe for this token. &lt;p&gt; A token may be wiped, removing and burning tokens from a specific account, by a token wipe transaction, which MUST be signed by this key. The `treasury_account` cannot be subjected to a token wipe. A token burn transaction, signed by the `supply_key`, serves to burn tokens held by the `treasury_account` instead.&lt;br/&gt; If this key is not set, the token cannot be wiped, and any transaction attempting to wipe the token from an account SHALL NOT succeed.&lt;br/&gt; This key MAY be set when the token is created, and MAY be set or modified via a token update transaction signed by the `admin_key`.&lt;br/&gt; If `admin_key` is not set, this value, whether set or unset, SHALL be immutable. |
| supply_key | [Key](#proto-Key) |  | Access control of token mint/burn for this token. &lt;p&gt; A token mint transaction MUST be signed by this key, and any token mint transaction not signed by the current `supply_key` for that token SHALL NOT succeed.&lt;br/&gt; A token burn transaction MUST be signed by this key, and any token burn transaction not signed by the current `supply_key` for that token SHALL NOT succeed.&lt;br/&gt; This key MAY be set when the token is created, and MAY be set or modified via a token update transaction signed by the `admin_key`.&lt;br/&gt; If `admin_key` is not set, this value, whether set or unset, SHALL be immutable. |
| fee_schedule_key | [Key](#proto-Key) |  | Access control of the `custom_fees` field for this token. &lt;p&gt; The token custom fee schedule may be changed, modifying the fees charged for transferring that token, by a token update transaction, which MUST be signed by this key.&lt;br/&gt; If this key is not set, the token custom fee schedule cannot be changed, and any transaction attempting to change the custom fee schedule for this token SHALL NOT succeed.&lt;br/&gt; This key MAY be set when the token is created, and MAY be set or modified via a token update transaction signed by the `admin_key`.&lt;br/&gt; If `admin_key` is not set, this value, whether set or unset, SHALL be immutable. |
| pause_key | [Key](#proto-Key) |  | Access control of pause/unpause for this token. &lt;p&gt; A token may be paused, preventing any transaction from transferring that token, by a token update transaction signed by this key.&lt;br/&gt; If this key is not set, the token cannot be paused, and any transaction attempting to pause the token SHALL NOT succeed.&lt;br/&gt; This key MAY be set when the token is created, and MAY be set or modified via a token update transaction signed by the `admin_key`.&lt;br/&gt; If `admin_key` is not set, this value, whether set or unset, SHALL be immutable. |
| last_used_serial_number | [int64](#int64) |  | A last used serial number for this token. &lt;p&gt; This SHALL apply only to non-fungible tokens.&lt;br/&gt; When a new NFT is minted, the serial number to apply SHALL be calculated from this value. |
| deleted | [bool](#bool) |  | A flag indicating that this token is deleted. &lt;p&gt; A transaction involving a deleted token MUST NOT succeed. |
| token_type | [TokenType](#proto-TokenType) |  | A type for this token. &lt;p&gt; A token SHALL be either `FUNGIBLE_COMMON` or `NON_FUNGIBLE_UNIQUE`.&lt;br/&gt; If this value was omitted during token creation, `FUNGIBLE_COMMON` SHALL be used. |
| supply_type | [TokenSupplyType](#proto-TokenSupplyType) |  | A supply type for this token. &lt;p&gt; A token SHALL have either `INFINITE` or `FINITE` supply type.&lt;br/&gt; If this value was omitted during token creation, the value `INFINITE` SHALL be used. |
| auto_renew_account_id | [AccountID](#proto-AccountID) |  | An identifier for the account (if any) that the network will attempt to charge for this token&#39;s auto-renewal upon expiration. &lt;p&gt; This field is OPTIONAL. If it is not set then renewal fees SHALL be charged to the account identified by `treasury_account_id`. |
| auto_renew_seconds | [int64](#int64) |  | A number of seconds by which the network should automatically extend this token&#39;s expiration. &lt;p&gt; If the token has a valid auto-renew account, and is not deleted upon expiration, the network SHALL attempt to automatically renew this token.&lt;br/&gt; If this is not provided in an allowed range on token creation, the transaction SHALL fail with `INVALID_AUTO_RENEWAL_PERIOD`.&lt;br/&gt; The default values for the minimum period and maximum period are 30 days and 90 days, respectively. |
| expiration_second | [int64](#int64) |  | An expiration time for this token, in seconds since the epoch. &lt;p&gt; For this purpose, `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`. |
| memo | [string](#string) |  | A short description of this token. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| max_supply | [int64](#int64) |  | A maximum supply of this token.&lt;br/&gt; This is the maximum number of tokens of this type that may be issued. &lt;p&gt; This limit SHALL apply regardless of `token_type`.&lt;br/&gt; If `supply_type` is `INFINITE` then this value MUST be 0.&lt;br/&gt; If `supply_type` is `FINITE`, then this value MUST be greater than 0. |
| paused | [bool](#bool) |  | A flag indicating that this token is paused. &lt;p&gt; A transaction involving a paused token, other than token_unpause, MUST NOT succeed. |
| accounts_frozen_by_default | [bool](#bool) |  | A flag indicating that accounts associated to this token are frozen by default. &lt;p&gt; Accounts newly associated with this token CANNOT transact in the token until unfrozen.&lt;br/&gt; This SHALL NOT prevent a `tokenReject` transaction to return the tokens from an account to the treasury account. |
| accounts_kyc_granted_by_default | [bool](#bool) |  | A flag indicating that accounts associated with this token are granted KYC by default. |
| custom_fees | [CustomFee](#proto-CustomFee) | repeated | A custom fee schedule for this token. |
| metadata | [bytes](#bytes) |  | A Token &#34;Metadata&#34;. &lt;p&gt; This value, if set, SHALL NOT exceed 100 bytes. |
| metadata_key | [Key](#proto-Key) |  | Access Control of metadata update for this token. &lt;p&gt; A transaction to update the `metadata` field of this token MUST be signed by this key.&lt;br/&gt; If this token is a non-fungible/unique token type, a transaction to update the `metadata` field of any individual serialized unique token of this type MUST be signed by this key.&lt;br/&gt; If this key is not set, the token metadata SHALL NOT be changed after it is created.&lt;br/&gt; If this key is not set, the metadata for any individual serialized token of this type SHALL NOT be changed after it is created.&lt;br/&gt; This key MAY be set when the token is created, and MAY be set or modified via a token update transaction signed by the `admin_key`.&lt;br/&gt; If `admin_key` is not set, this value, whether set or unset, SHALL be immutable. |





 

 

 

 



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

