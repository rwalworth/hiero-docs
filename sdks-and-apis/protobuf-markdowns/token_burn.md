# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_burn.proto](#services_token_burn-proto)
    - [TokenBurnTransactionBody](#proto-TokenBurnTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_burn-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_burn.proto
# Token Burn
Permanently remove tokens from circulation, akin to how a fiat treasury
will physically burn worn out bank notes.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenBurnTransactionBody"></a>

### TokenBurnTransactionBody
Burns tokens from the Token&#39;s treasury Account.

The token MUST have a `supply_key` set and that key MUST NOT
be an empty `KeyList`.&lt;br/&gt;
The token `supply_key` MUST sign this transaction.&lt;br/&gt;
This operation SHALL decrease the total supply for the token type by
the number of tokens &#34;burned&#34;.&lt;br/&gt;
The total supply for the token type MUST NOT be reduced below zero (`0`)
by this transaction.&lt;br/&gt;
The tokens to burn SHALL be deducted from the token treasury account.&lt;br/&gt;
If the token is a fungible/common type, the amount MUST be specified.&lt;br/&gt;
If the token is a non-fungible/unique type, the specific serial numbers
MUST be specified.&lt;br/&gt;
The global batch size limit (`tokens.nfts.maxBatchSizeBurn`) SHALL set
the maximum number of individual NFT serial numbers permitted in a single
`tokenBurn` transaction.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token | [TokenID](#proto-TokenID) |  | A token identifier. &lt;p&gt; This SHALL identify the token type to &#34;burn&#34;.&lt;br/&gt; The identified token MUST exist, and MUST NOT be deleted. |
| amount | [uint64](#uint64) |  | An amount to burn from the Treasury Account. &lt;p&gt; This is interpreted as an amount in the smallest possible denomination for the token (10&lt;sup&gt;-decimals&lt;/sup&gt; whole tokens).&lt;br/&gt; The balance for the token treasury account MUST contain sufficient tokens to complete this transaction with a non-negative balance.&lt;br/&gt; If this value is equal to zero (`0`), the token SHOULD be a non-fungible/unique type.&lt;br/&gt; If this value is non-zero, the token MUST be a fungible/common type. |
| serialNumbers | [int64](#int64) | repeated | A list of serial numbers to burn from the Treasury Account. &lt;p&gt; This list MUST NOT contain more entries than the current limit set by the network configuration value `tokens.nfts.maxBatchSizeBurn`.&lt;br/&gt; The treasury account for the token MUST hold each unique token identified in this list.&lt;br/&gt; If this list is not empty, the token MUST be a non-fungible/unique type.&lt;br/&gt; If this list is empty, the token MUST be a fungible/common type. |





 

 

 

 



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

