# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_reject.proto](#services_token_reject-proto)
    - [TokenReference](#proto-TokenReference)
    - [TokenRejectTransactionBody](#proto-TokenRejectTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_reject-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_reject.proto
# Token Reject
Messages used to implement a transaction to reject a token type from an
account.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenReference"></a>

### TokenReference
A union token identifier.

Identify a fungible/common token type, or a single
non-fungible/unique token serial.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fungible_token | [TokenID](#proto-TokenID) |  | A fungible/common token type. |
| nft | [NftID](#proto-NftID) |  | A single specific serialized non-fungible/unique token. |






<a name="proto-TokenRejectTransactionBody"></a>

### TokenRejectTransactionBody
Reject undesired token(s).&lt;br/&gt;
Transfer one or more token balances held by the requesting account to the
treasury for each token type.

Each transfer SHALL be one of the following
- A single non-fungible/unique token.
- The full balance held for a fungible/common token.
A single `tokenReject` transaction SHALL support a maximum
of 10 transfers.&lt;br/&gt;
A token that is `pause`d MUST NOT be rejected.&lt;br/&gt;
If the `owner` account is `frozen` with respect to the identified token(s)
the token(s) MUST NOT be rejected.&lt;br/&gt;
The `payer` for this transaction, and `owner` if set, SHALL NOT be charged
any custom fees or other fees beyond the `tokenReject` transaction fee.

### Block Stream Effects
- Each successful transfer from `payer` to `treasury` SHALL be recorded in
  the `token_transfer_list` for the transaction record.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| owner | [AccountID](#proto-AccountID) |  | An account identifier.&lt;br/&gt; This OPTIONAL field identifies the account holding the tokens to be rejected. &lt;p&gt; If set, this account MUST sign this transaction. If not set, the `payer` for this transaction SHALL be the effective `owner` for this transaction. |
| rejections | [TokenReference](#proto-TokenReference) | repeated | A list of one or more token rejections. &lt;p&gt; On success each rejected token serial number or balance SHALL be transferred from the requesting account to the treasury account for that token type.&lt;br/&gt; After rejection the requesting account SHALL continue to be associated with the token.&lt;br/&gt; If dissociation is desired then a separate `TokenDissociate` transaction MUST be submitted to remove the association.&lt;br/&gt; This list MUST contain at least one (1) entry and MUST NOT contain more than ten (10) entries. |





 

 

 

 



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

