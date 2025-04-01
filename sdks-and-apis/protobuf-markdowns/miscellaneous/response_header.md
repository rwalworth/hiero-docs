# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/response_header.proto](#services_response_header-proto)
    - [ResponseHeader](#proto-ResponseHeader)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_response_header-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/response_header.proto
# Response Header
A standard header for all query responses.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ResponseHeader"></a>

### ResponseHeader
A standard header returned with every query response.

The fields for `cost` or `stateProof` MAY be unset if the requested
`ResponseType` does not request those values.&lt;br/&gt;
The `responseType` SHALL match the request response type.&lt;br/&gt;
The `nodeTransactionPrecheckCode` field SHALL contain the result code
for the query.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| nodeTransactionPrecheckCode | [ResponseCodeEnum](#proto-ResponseCodeEnum) |  | The result code for this query. &lt;p&gt; This value SHALL indicate either success or the reason for failure. |
| responseType | [ResponseType](#proto-ResponseType) |  | The response type requested for this query. &lt;p&gt; This SHALL be the response type requested in the query header. |
| cost | [uint64](#uint64) |  | Requested cost estimate.&lt;br/&gt; This is the fee that _would be_ charged if the query was executed. &lt;p&gt; This value SHALL be set if the response type requested requires cost information, and SHALL NOT be set otherwise.&lt;br/&gt; This value SHALL include the query fee, but SHALL NOT include the transfer fee required to execute the fee payment transaction. |
| stateProof | [bytes](#bytes) |  | A state proof for the information requested.

This field SHALL NOT be set if the response type does not require a state proof.&lt;br/&gt; This field SHALL NOT be set if a state proof is not available for the query type.&lt;br/&gt; This field SHALL be set if the response type requested a state proof and a state proof is available. |





 

 

 

 



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

