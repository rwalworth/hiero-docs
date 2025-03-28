# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/blockrecords/block_info.proto](#services_state_blockrecords_block_info-proto)
    - [BlockInfo](#proto-BlockInfo)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_blockrecords_block_info-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/blockrecords/block_info.proto
# Block Info
Information about the most recent block; including block hashes, current and
previous start times, etc...

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-BlockInfo"></a>

### BlockInfo
Information for a transaction block.

This includes:
- last block number.
- consensus times for:
   - previous block start.
   - current block start.
   - last handled transaction.
- hash data for a rolling window of 256 blocks.
- whether migration records were produced.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| last_block_number | [int64](#int64) |  | A block number. &lt;p&gt; The block number of the last completed immutable block. |
| first_cons_time_of_last_block | [Timestamp](#proto-Timestamp) |  | A consensus timestamp. &lt;p&gt; The consensus time of the first transaction for the last completed immutable block. |
| block_hashes | [bytes](#bytes) |  | A list of the last 256 block hashes.&lt;br/&gt; This is the SHA384 48 byte hashes of the previous 256 blocks, collected in a single byte array. &lt;p&gt; The first 48 bytes SHALL be the oldest block in the list.&lt;br/&gt; The last 48 bytes SHALL be the newest block, which is the last fully completed immutable block.&lt;br/&gt; This SHALL contain less than 256 block hashes if there are less than 256 preceding blocks; for instance, shortly after network genesis the network will not have processed 256 blocks yet. &lt;p&gt; This MAY change significantly for Block Stream (HIP TBD). |
| cons_time_of_last_handled_txn | [Timestamp](#proto-Timestamp) |  | A consensus timestamp. &lt;p&gt; The consensus time of the last transaction that was handled by the node within the current block.&lt;br/&gt; This property is how we &#39;advance the consensus clock&#39;. The node MUST continually set this property to the consensus timestamp for the most recent transaction completed by the node. |
| migration_records_streamed | [bool](#bool) |  | A flag indicating that migration records have been published. &lt;p&gt; This property SHALL be `false` immediately following a node upgrade&lt;br/&gt; This SHALL be set &#39;true&#39; as migration records, if any, are published. Migration records are typically published &#34;during&#34; the first transaction handled by the node following startup. |
| first_cons_time_of_current_block | [Timestamp](#proto-Timestamp) |  | A consensus timestamp. &lt;p&gt; The consensus time of the first transaction in the current block; necessary for reconnecting nodes to detect when the current block is finished. |





 

 

 

 



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

