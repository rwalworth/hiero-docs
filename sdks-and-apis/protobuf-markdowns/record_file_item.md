# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/record_file_item.proto](#block_stream_record_file_item-proto)
    - [RecordFileItem](#com-hedera-hapi-block-stream-RecordFileItem)
    - [RecordFileSignature](#com-hedera-hapi-block-stream-RecordFileSignature)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_record_file_item-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/record_file_item.proto
# Record File Block
This block carries the data from &#34;record stream&#34; and &#34;sidecar&#34;
files that preceded the block stream. Record blocks are full blocks,
not block items, but do not have a block header or block proof.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-RecordFileItem"></a>

### RecordFileItem
A Block Item for record files.

A `RecordFileItem` contains data produced before the innovation of the
Block Stream, when data was stored in files and validated by individual
signature files rather than a block proof.&lt;br/&gt;
This item enables a single format, the Block Stream, to carry both
historical and current data; eliminating the need to search two sources for
block and block chain data.&lt;br/&gt;
Any block containing this item requires special handling.
- The block SHALL have a `BlockHeader`.
   - Some fields in the `BlockHeader` may be interpreted differently, and
     may depend on when the original record file was created.
- The block SHALL NOT have a `BlockProof`.
- The block SHALL end with an `AddressBookProof`, which is only used for
  `RecordFileItem` blocks.
- The block SHALL contain _exactly one_ `RecordFileItem`.
- The block SHALL NOT contain any content item other than a `RecordFileItem`.
- The content of the `RecordFileItem` MUST be validated using the
  signature data and content provided herein according to the
  process used for Record Files prior to the creation of Block Stream.
   - This block item only replaces the requirement to read several
     individual files from cloud storage services.
- The address book relevant to a particular record file SHALL be available
  separately as an `AddressBookProof` item.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| creation_time | [proto.Timestamp](#proto-Timestamp) |  | The consensus time the record file was produced for.&lt;br/&gt; This comes from the record file name. |
| record_file_contents | [bytes](#bytes) |  | The contents of a record file.&lt;br/&gt; The first 4 bytes are a 32bit int little endian version number. The versions that existed are 2,3,5 and 6. |
| sidecar_file_contents | [proto.SidecarFile](#proto-SidecarFile) | repeated | The contents of sidecar files for this block.&lt;br/&gt; Each block can have zero or more sidecar files. |
| record_file_signatures | [RecordFileSignature](#com-hedera-hapi-block-stream-RecordFileSignature) | repeated | A collection of RSA signatures from consensus nodes.&lt;br/&gt; These signatures validate the hash of the record_file_contents field. |






<a name="com-hedera-hapi-block-stream-RecordFileSignature"></a>

### RecordFileSignature
A signature by a node on the SHA384 hash of the record file.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| signatures_bytes | [bytes](#bytes) |  | A single RSA signature.&lt;br/&gt; This is the RSA signature of the node on the SHA384 hash of the record file |
| node_id | [int32](#int32) |  | A unique node identifier.&lt;br/&gt; This is the node id of the consensus node that created this signature. |





 

 

 

 



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

