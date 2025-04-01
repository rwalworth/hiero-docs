# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/output/block_header.proto](#block_stream_output_block_header-proto)
    - [BlockHeader](#com-hedera-hapi-block-stream-output-BlockHeader)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_output_block_header-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/output/block_header.proto
# Block Header
The block header reports information required to correctly process a block.
This includes versions, block number, and algorithms used.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-output-BlockHeader"></a>

### BlockHeader
A Block Header.

Each block in the block stream SHALL begin with a block header.&lt;br/&gt;
The block header SHALL provide the base minimum information needed to
correctly interpret and process that block, or stop processing
if appropriate.&lt;br/&gt;
The block header MUST describe, at minimum, the following items.
 - The version of the block stream data
 - The block number
 - The hash of the previous block
 - The hash algorithm used to generate the block hash

All fields of this message are REQUIRED, with the exception that
`hash_algorithm` MAY be _transmitted_ as a default value to improve
data efficiency.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hapi_proto_version | [proto.SemanticVersion](#proto-SemanticVersion) |  | A version of the HAPI specification.&lt;br/&gt; This is the API version that was used to serialize the block. |
| software_version | [proto.SemanticVersion](#proto-SemanticVersion) |  | A version of the consensus node software.&lt;br/&gt; This is the software version that executed the transactions within this block. |
| number | [uint64](#uint64) |  | A block number for this block. &lt;p&gt; This value MUST be exactly `1` more than the previous block.&lt;br/&gt; Client systems SHOULD optimistically reject any block with a gap or reverse in `number` sequence, and MAY assume the block stream has encountered data loss, data corruption, or unauthorized modification. |
| first_transaction_consensus_time | [proto.Timestamp](#proto-Timestamp) |  | A consensus timestamp for the start of this block. &lt;p&gt; This SHALL be the timestamp assigned by the hashgraph consensus algorithm to the first transaction of this block. |
| hash_algorithm | [proto.BlockHashAlgorithm](#proto-BlockHashAlgorithm) |  | A hash algorithm used for this block, including the block proof. &lt;p&gt; This SHOULD always be `SHA2_384`, currently. |





 

 

 

 



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

