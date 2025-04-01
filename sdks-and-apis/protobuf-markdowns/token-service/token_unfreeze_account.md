# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_unfreeze_account.proto](#services_token_unfreeze_account-proto)
    - [TokenUnfreezeAccountTransactionBody](#proto-TokenUnfreezeAccountTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_unfreeze_account-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_unfreeze_account.proto
# Token Unfreeze
Release a freeze on tokens of an identified type for an identified account.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenUnfreezeAccountTransactionBody"></a>

### TokenUnfreezeAccountTransactionBody
Resume transfers of a token type for an account.&lt;br/&gt;
This releases previously frozen assets of one account with respect to
one token type. Once unfrozen, that account can once again send or
receive tokens of the identified type.

The token MUST have a `freeze_key` set and that key MUST NOT
be an empty `KeyList`.&lt;br/&gt;
The token `freeze_key` MUST sign this transaction.&lt;br/&gt;
The identified token MUST exist, MUST NOT be deleted, MUST NOT be paused,
and MUST NOT be expired.&lt;br/&gt;
The identified account MUST exist, MUST NOT be deleted, and
MUST NOT be expired.&lt;br/&gt;
If the identified account is not frozen with respect to the identified
token, the transaction SHALL succeed, but no change SHALL be made.&lt;br/&gt;
An association between the identified account and the identified
token MUST exist.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token | [TokenID](#proto-TokenID) |  | A token identifier. &lt;p&gt; This SHALL identify the token type to &#34;unfreeze&#34;.&lt;br/&gt; The identified token MUST exist, MUST NOT be deleted, and MUST be associated to the identified account. |
| account | [AccountID](#proto-AccountID) |  | An account identifier. &lt;p&gt; This shall identify the account to &#34;unfreeze&#34;.&lt;br/&gt; The identified account MUST exist, MUST NOT be deleted, MUST NOT be expired, and MUST be associated to the identified token.&lt;br/&gt; The identified account SHOULD be &#34;frozen&#34; with respect to the identified token. |





 

 

 

 



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

