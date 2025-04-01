# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/query_header.proto](#services_query_header-proto)
    - [QueryHeader](#proto-QueryHeader)
  
    - [ResponseType](#proto-ResponseType)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_query_header-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/query_header.proto
# Query Header
Messages that comprise a header sent with each query request.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-QueryHeader"></a>

### QueryHeader
A standard query header.&lt;br/&gt;
Each query from the client to the node must contain a QueryHeader, which
specifies the desired response type, and includes a payment transaction
that will compensate the network for responding to the query.
The payment may be blank if the query is free.

The payment transaction MUST be a `cryptoTransfer` from the payer account
to the account of the node where the query is submitted.&lt;br/&gt;
If the payment is sufficient, the network SHALL respond with the response
type requested.&lt;br/&gt;
If the response type is `COST_ANSWER` the payment MUST be unset.
A state proof SHALL be available for some types of information.&lt;br/&gt;
A state proof SHALL be available for a Record, but not a receipt, and the
response entry for each supported &#34;get info&#34; query.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| payment | [Transaction](#proto-Transaction) |  | A signed `CryptoTransferTransaction` to pay query fees. &lt;p&gt; This MUST transfer HBAR from the &#34;payer&#34; to the responding node account sufficient to pay the query fees. |
| responseType | [ResponseType](#proto-ResponseType) |  | A type of query response requested. |





 


<a name="proto-ResponseType"></a>

### ResponseType
The type of query response.&lt;br/&gt;

This SHALL be answer-only as a default.&lt;br/&gt;
This value SHALL support an &#34;estimated cost&#34; type.&lt;br/&gt;
This value SHOULD support a &#34;state proof&#34; type, when available.

| Name | Number | Description |
| ---- | ------ | ----------- |
| ANSWER_ONLY | 0 | A response with the query answer. |
| ANSWER_STATE_PROOF | 1 | A response with both the query answer and a state proof. |
| COST_ANSWER | 2 | A response with the estimated cost to answer the query. |
| COST_ANSWER_STATE_PROOF | 3 | A response with the estimated cost to answer and a state proof. |


 

 

 



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

