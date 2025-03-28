# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/blockstream/block_stream_info.proto](#services_state_blockstream_block_stream_info-proto)
    - [BlockStreamInfo](#com-hedera-hapi-node-state-blockstream-BlockStreamInfo)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_blockstream_block_stream_info-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/blockstream/block_stream_info.proto
# Block Stream Info
Information stored in consensus state at the beginning of each block to
record the status of the immediately prior block.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-node-state-blockstream-BlockStreamInfo"></a>

### BlockStreamInfo
A message stored in state to maintain block stream parameters.&lt;br/&gt;
Nodes use this information for three purposes.
1. To maintain hash chain continuity at restart and reconnect boundaries.
1. To store historical hashes for implementation of the EVM `BLOCKHASH`
   and `PREVRANDAO` opcodes.
1. To track the amount of consensus time that has passed between blocks.

This value MUST be updated for every block.&lt;br/&gt;
This value MUST be transmitted in the &#34;state changes&#34; section of
_each_ block, but MUST be updated at the beginning of the _next_ block.&lt;br/&gt;
This value SHALL contain the block hash up to, and including, the
immediately prior completed block.&lt;br/&gt;
The state change to update this singleton MUST be the last
block item in this block.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block_number | [uint64](#uint64) |  | A block number.&lt;br/&gt; This is the current block number. |
| block_time | [proto.Timestamp](#proto-Timestamp) |  | A consensus time for the current block.&lt;br/&gt; This is the consensus time of the first round in the current block, and is used to determine if this block was the first across an important boundary in consensus time, such as UTC midnight. This may also be used to purge entities expiring between the last block time and this time. |
| trailing_output_hashes | [bytes](#bytes) |  | A concatenation of hash values.&lt;br/&gt; This combines several trailing output block item hashes and is used as a seed value for a pseudo-random number generator.&lt;br/&gt; This is also required to implement the EVM `PREVRANDAO` opcode.&lt;br/&gt; This MUST contain at least 256 bits of entropy. |
| trailing_block_hashes | [bytes](#bytes) |  | A concatenation of hash values.&lt;br/&gt; This field combines up to 256 trailing block hashes. &lt;p&gt; If this message is for block number N, then the earliest available hash SHALL be for block number N-256.&lt;br/&gt; The latest available hash SHALL be for block N-1.&lt;br/&gt; This is REQUIRED to implement the EVM `BLOCKHASH` opcode. &lt;p&gt; ### Field Length Each hash value SHALL be the trailing 265 bits of a SHA2-384 hash.&lt;br/&gt; The length of this field SHALL be an integer multiple of 32 bytes.&lt;br/&gt; This field SHALL be at least 32 bytes.&lt;br/&gt; The maximum length of this field SHALL be 8192 bytes. |
| input_tree_root_hash | [bytes](#bytes) |  | A SHA2-384 hash value.&lt;br/&gt; This is the hash of the &#34;input&#34; subtree for this block. |
| start_of_block_state_hash | [bytes](#bytes) |  | A SHA2-384 hash value.&lt;br/&gt; This is the hash of consensus state at the _start_ of this block. |
| num_preceding_output_items | [uint32](#uint32) |  | A count of &#34;output&#34; block items in this block. &lt;p&gt; This SHALL count the number of output block items that _precede_ the state change that updates this singleton. |
| rightmost_preceding_output_tree_hashes | [bytes](#bytes) | repeated | A concatenation of SHA2-384 hash values.&lt;br/&gt; This is the &#34;rightmost&#34; values of the &#34;output&#34; subtree. &lt;p&gt; The subtree containing these hashes SHALL be constructed from all &#34;output&#34; `BlockItem`s in this block that _precede_ the update to this singleton. |
| block_end_time | [proto.Timestamp](#proto-Timestamp) |  | A block-end consensus time stamp. &lt;p&gt; This field SHALL hold the last-used consensus time for the current block. |
| post_upgrade_work_done | [bool](#bool) |  | Whether the post-upgrade work has been done. &lt;p&gt; This MUST be false if and only if the network just restarted after an upgrade and has not yet done the post-upgrade work. |
| creation_software_version | [proto.SemanticVersion](#proto-SemanticVersion) |  | A version describing the version of application software. &lt;p&gt; This SHALL be the software version that created this block. |
| last_interval_process_time | [proto.Timestamp](#proto-Timestamp) |  | The time stamp at which the last interval process was done. &lt;p&gt; This field SHALL hold the consensus time for the last time at which an interval of time-dependent events were processed. |
| last_handle_time | [proto.Timestamp](#proto-Timestamp) |  | The time stamp at which the last user transaction was handled. &lt;p&gt; This field SHALL hold the consensus time for the last time at which a user transaction was handled. |





 

 

 

 



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

