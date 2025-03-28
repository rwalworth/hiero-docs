# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_wipe_account.proto](#services_token_wipe_account-proto)
    - [TokenWipeAccountTransactionBody](#proto-TokenWipeAccountTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_wipe_account-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_wipe_account.proto
# Token Wipe Account
Administratively burn tokens owned by a single, non-treasury, account.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenWipeAccountTransactionBody"></a>

### TokenWipeAccountTransactionBody
Wipe (administratively burn) tokens held by a non-treasury account.&lt;br/&gt;
On success, the requested tokens will be removed from the identified account
and the token supply will be reduced by the amount &#34;wiped&#34;.

This transaction MUST be signed by the token `wipe_key`.&lt;br/&gt;
The identified token MUST exist, MUST NOT be deleted,
and MUST NOT be paused.&lt;br/&gt;
The identified token MUST have a valid `Key` set for the `wipe_key` field,
and that key MUST NOT be an empty `KeyList`.&lt;br/&gt;
The identified account MUST exist, MUST NOT be deleted, MUST be
associated to the identified token, MUST NOT be frozen for the identified
token, MUST NOT be the token `treasury`, and MUST hold a balance for the
token or the specific serial numbers provided.&lt;br/&gt;
This transaction SHOULD provide a value for `amount` or `serialNumbers`,
but MUST NOT set both fields.

### Block Stream Effects
The new total supply for the wiped token type SHALL be recorded.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token | [TokenID](#proto-TokenID) |  | A token identifier. &lt;p&gt; This field is REQUIRED.&lt;br/&gt; The identified token MUST exist, MUST NOT be paused, MUST NOT be deleted, and MUST NOT be expired. |
| account | [AccountID](#proto-AccountID) |  | An account identifier.&lt;br/&gt; This identifies the account from which tokens will be wiped. &lt;p&gt; This field is REQUIRED.&lt;br/&gt; The identified account MUST NOT be deleted or expired.&lt;br/&gt; If the identified token `kyc_key` is set to a valid key, the identified account MUST have &#34;KYC&#34; granted.&lt;br/&gt; The identified account MUST NOT be the `treasury` account for the identified token. |
| amount | [uint64](#uint64) |  | An amount of fungible/common tokens to wipe. &lt;p&gt; If the identified token is a non-fungible/unique token type, this value MUST be exactly zero(`0`).&lt;br/&gt; If the identified token type is fungible/common: &lt;ul&gt; &lt;li&gt;This value SHALL be specified in units of the smallest denomination possible for the identified token (&lt;tt&gt;10&lt;sup&gt;-decimals&lt;/sup&gt;&lt;/tt&gt; whole tokens).&lt;/li&gt; &lt;li&gt;This value MUST be strictly less than `Long.MAX_VALUE`.&lt;/li&gt; &lt;li&gt;This value MUST be less than or equal to the current total supply for the identified token.&lt;/li&gt; &lt;li&gt;This value MUST be less than or equal to the current balance held by the identified account.&lt;/li&gt; &lt;li&gt;This value MAY be zero(`0`).&lt;/li&gt; &lt;/ul&gt; |
| serialNumbers | [int64](#int64) | repeated | A list of serial numbers to wipe.&lt;br/&gt; The non-fungible/unique tokens with these serial numbers will be destroyed and cannot be recovered or reused. &lt;p&gt; If the identified token type is a fungible/common type, this list MUST be empty.&lt;br/&gt; If the identified token type is non-fungible/unique: &lt;ul&gt; &lt;li&gt;This list MUST contain at least one entry if the identified token type is non-fungible/unique.&gt;/li&gt; &lt;li&gt;This list MUST NOT contain more entries than the current total supply for the identified token.&lt;/li&gt; &lt;li&gt;Every entry in this list MUST be a valid serial number for the identified token (i.e. &#34;collection&#34;).&lt;/li&gt; &lt;li&gt;Every entry in this list MUST be owned by the identified account&lt;/li&gt; &lt;li&gt;&lt;/li&gt; &lt;/ul&gt; This list MUST NOT contain more entries than the network configuration value for batch size limit, typically ten(`10`). |





 

 

 

 



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

