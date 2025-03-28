# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/transaction_response.proto](#services_transaction_response-proto)
    - [TransactionResponse](#proto-TransactionResponse)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_transaction_response-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/transaction_response.proto
# Transaction Response
Message(s) sent in response to submitting a transaction.
The response(s) detailed here SHALL only represent that the transaction
was received and checked by the single node to which it was submitted.&lt;br/&gt;
To obtain the result from _network consensus_, a client MUST submit a
`getTransactionReceipts` query.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TransactionResponse"></a>

### TransactionResponse
A message sent by a node in response to a transaction submission.&lt;br/&gt;
This message only acknowledges that the individual node has checked
the transaction, completed pre-check, and checked the fee offered.

If the transaction fee is not sufficient, the `nodeTransactionPrecheckCode`
value SHALL be `INSUFFICIENT_TX_FEE` and the `cost` field SHALL be the
actual transaction fee, in tinybar, required.&lt;br/&gt;
If the client requires acknowledgement of the network consensus result
for a transaction, the client SHOULD request a transaction receipt or
detailed transaction record. A client MAY also obtain network consensus
results from a mirror node.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| nodeTransactionPrecheckCode | [ResponseCodeEnum](#proto-ResponseCodeEnum) |  | A pre-consensus response code. &lt;p&gt; This response SHALL represent the response of the individual node, and SHALL NOT represent the consensus of the network. |
| cost | [uint64](#uint64) |  | An approximate transaction fee. &lt;p&gt; This value SHALL be `0` unless the `nodeTransactionPrecheckCode` is `INSUFFICIENT_TX_FEE`.&lt;br/&gt; This value SHOULD be an amount, in tinybar, that _would have_ succeeded at the time the transaction was submitted.&lt;br/&gt; Note that this amount is not guaranteed to succeed in a future transaction due to uncontrolled variables, such as network congestion, but should be considered a close approximation. |





 

 

 

 



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

