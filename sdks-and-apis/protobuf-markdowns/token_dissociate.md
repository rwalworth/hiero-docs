# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_dissociate.proto](#services_token_dissociate-proto)
    - [TokenDissociateTransactionBody](#proto-TokenDissociateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_dissociate-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_dissociate.proto
# Token Dissociate
Remove association between an account and one or more Hedera Token
Service (HTS) tokens.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenDissociateTransactionBody"></a>

### TokenDissociateTransactionBody
Dissociate an account from one or more HTS tokens.

If the identified account is not found,
the transaction SHALL return `INVALID_ACCOUNT_ID`.&lt;br/&gt;
If the identified account has been deleted,
the transaction SHALL return `ACCOUNT_DELETED`.&lt;br/&gt;
If any of the identified tokens is not found,
the transaction SHALL return `INVALID_TOKEN_REF`.&lt;br/&gt;
If any of the identified tokens has been deleted,
the transaction SHALL return `TOKEN_WAS_DELETED`.&lt;br/&gt;
If an association does not exist for any of the identified tokens,
the transaction SHALL return `TOKEN_NOT_ASSOCIATED_TO_ACCOUNT`.&lt;br/&gt;
If the identified account has a nonzero balance for any of the identified
tokens, and that token is neither deleted nor expired, the
transaction SHALL return `TRANSACTION_REQUIRES_ZERO_TOKEN_BALANCES`.&lt;br/&gt;
If one of the identified tokens is a fungible/common token that is expired,
the account MAY disassociate from that token, even if that token balance is
not zero for that account.&lt;br/&gt;
If one of the identified tokens is a non-fungible/unique token that is
expired, the account MUST NOT disassociate if that account holds any
individual NFT of that token. In this situation the transaction SHALL
return `TRANSACTION_REQUIRED_ZERO_TOKEN_BALANCES`.&lt;br/&gt;
The identified account MUST sign this transaction.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account | [AccountID](#proto-AccountID) |  | An account identifier. &lt;p&gt; The identified account SHALL be dissociated from each of the tokens identified in the `tokens` field. This field is REQUIRED and MUST be a valid account identifier.&lt;br/&gt; The identified account MUST exist in state.&lt;br/&gt; The identified account MUST NOT be deleted.&lt;br/&gt; The identified account MUST NOT be expired. |
| tokens | [TokenID](#proto-TokenID) | repeated | A list of token identifiers. &lt;p&gt; Each token identified in this list SHALL be dissociated from the account identified in the `account` field.&lt;br/&gt; This list MUST NOT be empty. Each entry in this list MUST be a valid token identifier.&lt;br/&gt; Each entry in this list MUST be currently associated to the account identified in `account`.&lt;br/&gt; Entries in this list MAY be expired, if the token type is fungible/common.&lt;br/&gt; Each entry in this list MUST NOT be deleted. |





 

 

 

 



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

