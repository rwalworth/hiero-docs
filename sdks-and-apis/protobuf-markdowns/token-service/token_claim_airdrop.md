# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_claim_airdrop.proto](#services_token_claim_airdrop-proto)
    - [TokenClaimAirdropTransactionBody](#proto-TokenClaimAirdropTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_claim_airdrop-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_claim_airdrop.proto
# Token Claim Airdrop
Messages used to implement a transaction to claim a pending airdrop.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenClaimAirdropTransactionBody"></a>

### TokenClaimAirdropTransactionBody
Token claim airdrop&lt;br/&gt;
Complete one or more pending transfers on behalf of the
recipient(s) for an airdrop.

The sender MUST have sufficient balance to fulfill the airdrop at the
time of claim. If the sender does not have sufficient balance, the
claim SHALL fail.&lt;br/&gt;
Each pending airdrop successfully claimed SHALL be removed from state and
SHALL NOT be available to claim again.&lt;br/&gt;
Each claim SHALL be represented in the transaction body and
SHALL NOT be restated in the record file.&lt;br/&gt;
All claims MUST succeed for this transaction to succeed.

### Block Stream Effects
The completed transfers SHALL be present in the transfer list.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| pending_airdrops | [PendingAirdropId](#proto-PendingAirdropId) | repeated | A list of one or more pending airdrop identifiers. &lt;p&gt; This transaction MUST be signed by the account identified by the `receiver_id` for each entry in this list.&lt;br/&gt; This list MUST contain between 1 and 10 entries, inclusive.&lt;br/&gt; This list MUST NOT have any duplicate entries. |





 

 

 

 



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

