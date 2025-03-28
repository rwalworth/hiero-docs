# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/block.proto](#block_stream_block-proto)
    - [Block](#com-hedera-hapi-block-stream-Block)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_block-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/block.proto
# Block Stream
The base element of the block stream _at rest_.
A `Block` contains a record of all transactions, results, and outputs for
a block in the chain. Each `Block` also contains a state proof for
validation and a header with version and algorithm information.

Block entries are not designed for streaming, but for storing blocks in
persistent storage, verifying block stream data, and as query responses
when a block is requested from a block node.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-Block"></a>

### Block
A single complete Hedera block chain block.

This is a single block structure and SHALL NOT represent the primary
mechanism to transmit a block stream.&lt;br/&gt;
The primary mechanism for transmitting block stream data SHALL be to
stream individual block items to the block node(s).&lt;br/&gt;
The only delimiter between blocks when streamed SHALL be the `BlockHeader`
item and `BlockProof` item.

This block SHALL be verifiable as correct using only data in the block,
including the `BlockProof`, and public keys for the consensus nodes.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| items | [BlockItem](#com-hedera-hapi-block-stream-BlockItem) | repeated | A list of items that, together, make up this block. &lt;p&gt; This list SHALL begin with a `BlockHeader`.&lt;br/&gt; This list SHALL end with a `BlockProof`.&lt;br/&gt; Items in this list SHALL be in exactly the same order produced by consensus.&lt;br/&gt; Items in this list MAY be filtered, if so requested.&lt;br/&gt; If this list is filtered, removed items SHALL be replaced with `FilteredBlockItem` entries.&lt;br/&gt; |





 

 

 

 



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

