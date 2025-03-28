# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [platform/event/state_signature_transaction.proto](#platform_event_state_signature_transaction-proto)
    - [StateSignatureTransaction](#com-hedera-hapi-platform-event-StateSignatureTransaction)
  
- [Scalar Value Types](#scalar-value-types)



<a name="platform_event_state_signature_transaction-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## platform/event/state_signature_transaction.proto
# State Signature Transaction
An signature of a state snapshot gossiped to other nodes.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119).


<a name="com-hedera-hapi-platform-event-StateSignatureTransaction"></a>

### StateSignatureTransaction
An signature of a state snapshot gossiped to other nodes.
Each node SHALL hash the root of the merkle tree of a state snapshot every
round. Once this hash is calculated, it SHOULD be signed with the nodes
private signing key. This signature, together with the hash SHOULD be added
to an event as a StateSignatureTransaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| round | [int64](#int64) |  | The round number corresponding to the round number of the state snapshot being signed.&lt;br/&gt; This number MUST be greater than 0. |
| signature | [bytes](#bytes) |  | The signature of state snapshot hash.&lt;br/&gt; This signature MUST be a RSA signature with a maximum length of 384 bytes.&lt;br/&gt; The signature algorithm used MUST be RSASSA-PKCS1-v1_5 with SHA-384. |
| hash | [bytes](#bytes) |  | The hash of the state snapshot being signed.&lt;br/&gt; This hash MUST be a SHA-384 hash. |





 

 

 

 



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

