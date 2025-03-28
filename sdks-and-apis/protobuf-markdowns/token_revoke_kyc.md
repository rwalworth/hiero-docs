# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_revoke_kyc.proto](#services_token_revoke_kyc-proto)
    - [TokenRevokeKycTransactionBody](#proto-TokenRevokeKycTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_revoke_kyc-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_revoke_kyc.proto
# Token Revoke KYC
Revoke &#34;KYC&#34; status from an account with respect to a token.

The &#34;KYC&#39; property is named for the &#34;Know Your Customer&#34; requirements in
US federal regulations (FINRA 2090 and related US Code) that was subsequently
incorporated into laws and regulations for many worldwide jurisdictions.
The process requires a regulated financial entity to positively identify
customers and certain other entities.

This transaction enables a token administrator to track whether KYC
requirements are met for a given account transacting in that token.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenRevokeKycTransactionBody"></a>

### TokenRevokeKycTransactionBody
Revoke &#34;Know Your Customer&#34;(KYC) from one account for a single token.

This transaction MUST be signed by the `kyc_key` for the token.&lt;br/&gt;
The identified token MUST have a `kyc_key` set to a valid `Key` value.&lt;br/&gt;
The token `kyc_key` MUST NOT be an empty `KeyList`.&lt;br/&gt;
The identified token MUST exist and MUST NOT be deleted.&lt;br/&gt;
The identified account MUST exist and MUST NOT be deleted.&lt;br/&gt;
The identified account MUST have an association to the identified token.&lt;br/&gt;
On success the association between the identified account and the identified
token SHALL NOT be marked as &#34;KYC granted&#34;.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token | [TokenID](#proto-TokenID) |  | A token identifier. &lt;p&gt; The identified token SHALL revoke &#34;KYC&#34; for the account identified by the `account` field.&lt;br/&gt; The identified token MUST be associated to the account identified by the `account` field. |
| account | [AccountID](#proto-AccountID) |  | An account identifier. &lt;p&gt; The token identified by the `token` field SHALL revoke &#34;KYC&#34; for the identified account.&lt;br/&gt; This account MUST be associated to the token identified by the `token` field. |





 

 

 

 



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

