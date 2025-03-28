# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/block_item.proto](#block_stream_block_item-proto)
    - [BlockItem](#com-hedera-hapi-block-stream-BlockItem)
    - [FilteredItemHash](#com-hedera-hapi-block-stream-FilteredItemHash)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_block_item-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/block_item.proto
# Block Item
A single item in the block stream, such as transaction data, event metadata,
or a a system transaction.&lt;br/&gt;
Each block consists of a block header, one or more block items,
and a block state proof. Within the block are a series of events delimited
by start_event block items.

This structure here MUST support a stream of block items with no enclosing
message.&lt;br/&gt;
Implementations SHOULD behave in a reasonable manner if used in a gRPC
bidirectional streaming RPC similar to
`rpc processBlocks(stream BlockItem) returns (stream Acknowledgement);`.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-BlockItem"></a>

### BlockItem
A single item within a block stream.

Each item in the block stream SHALL be self-contained and independent,
with the following constraints applicable to the _unfiltered_ stream.
- A block SHALL start with a `header`.
- A block SHALL end with a `state_proof`.
- A `block_header` SHALL be followed by an `event_header`.
- An `event_header` SHALL be followed by one or more
  `event_transaction` items.
- An `event_transaction` SHALL be followed by a `transaction_result`.
- A `transaction_result` MAY be followed by a `transaction_output`.
- A `transaction_result` (or a `transaction_output`, if present) MAY be
    followed by one or more `state_changes`.

This forms the following required sequence for each block, which is then
repeated within the block stream, indefinitely.  Note that there is no
container structure in the stream, the indentation below is only to
highlight repeated subsequences.&lt;br/&gt;
The order of items within each block below is REQUIRED and SHALL NOT change.

```text
header
  repeated {
    start_event
    repeated {
      event_transaction
      transaction_result
      (optional) transaction_output
      (optional) repeated state_changes
    }
  }
state_proof
```
A filtered stream may exclude some items above, depending on filter
criteria. A filtered item is replaced with a merkle path and hash value
to maintain block stream verifiability.

A BlockItem SHALL be individually and directly processed to create the
item hash.&lt;br/&gt;
Items to be hashed MUST NOT be contained within another item.&lt;br/&gt;
Items which might be filtered out of the stream MUST NOT be
contained in other items.

### Forward Compatibility
In order to maximize forward compatibility, and minimize the need to
coordinate deployments of different systems creating and processing
block streams in the future, the following rules SHALL be followed
for field numbering in this message.
- The first 15 field numbers SHALL be assigned to the fields present
  in the first release. Unused fields in this range SHALL remain reserved
  until needed for additional options that do not fit into &#34;input&#34; or
  &#34;output&#34; categories.
- Fields numbered 16 and above MUST be numbered as follows.
   - &#34;input&#34; items MUST use `odd` field numbers.
   - &#34;output&#34; items MUST use `even` field numbers.

#### Forward Compatibility Example
A future update adding three new items. A &#34;BlockTrailer&#34; item which is
neither input nor output, a new &#34;ConsensusTransom&#34; which is an input,
and a new &#34;BridgeTransform&#34; which is an output.
- The &#34;BlockTrailer&#34; is field 11, which is removed from the `reserved` list.
- The &#34;ConsensusTransom&#34; is an input, so it is field `17` (the first unused
  `odd` field greater than or equal to 16).
- The &#34;BridgeTransform&#34; is an output, so it is field `16` (the first unused
  even field greater than or equal to 16).

#### Initial Field assignment to &#34;input&#34;, &#34;output&#34;, and &#34;other&#34; categories.
- Inputs
   - `event_header`
   - `round_header`
   - `event_transaction`
- Outputs
   - `block_header`
   - `transaction_result`
   - `transaction_output`
   - `state_changes`
- Any subtree (depending on what was filtered).
  This item details it&#39;s path in the tree.
   - `filtered_item_hash`
- Neither input nor output (and not part of the &#34;proof&#34; merkle tree)
   - `block_proof`
   - `record_file`


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block_header | [output.BlockHeader](#com-hedera-hapi-block-stream-output-BlockHeader) |  | An header for the block, marking the start of a new block. |
| event_header | [input.EventHeader](#com-hedera-hapi-block-stream-input-EventHeader) |  | An header emitted at the start of a new network &#34;event&#34;. &lt;p&gt; This item SHALL contain the properties relevant to a single gossip event. |
| round_header | [input.RoundHeader](#com-hedera-hapi-block-stream-input-RoundHeader) |  | An header emitted at the start of a new consensus &#34;round&#34;. &lt;p&gt; This item SHALL contain the properties relevant to a single consensus round. |
| event_transaction | [com.hedera.hapi.platform.event.EventTransaction](#com-hedera-hapi-platform-event-EventTransaction) |  | A single transaction. &lt;p&gt; This item SHALL contain the serialized bytes of a single transaction.&lt;br/&gt; Each event transaction SHALL be either a `SignedTransaction` or an internal system-generated transaction.&lt;br/&gt; This item MUST NOT contain data for more than one `SignedTransaction` or system-generated transaction. |
| transaction_result | [output.TransactionResult](#com-hedera-hapi-block-stream-output-TransactionResult) |  | The result of running a transaction. &lt;p&gt; This item SHALL be present immediately after an `event_transaction` item.&lt;br/&gt; This item MAY be redacted in some circumstances, and SHALL be replaced with a `filtered_item` if removed. |
| transaction_output | [output.TransactionOutput](#com-hedera-hapi-block-stream-output-TransactionOutput) |  | A transaction output. &lt;p&gt; This item MAY not be present if a transaction does not produce an output.&lt;br/&gt; If a transaction does produce an output that is not reflected in state changes, then this item MUST be present after the `transaction_result` for that transaction. |
| state_changes | [output.StateChanges](#com-hedera-hapi-block-stream-output-StateChanges) |  | A set of state changes. &lt;p&gt; All changes to values in network state SHALL be described by stream items of this type.&lt;br/&gt; The source of these state changes SHALL be described by the `reason` enumeration. |
| filtered_item_hash | [FilteredItemHash](#com-hedera-hapi-block-stream-FilteredItemHash) |  | Verification data for an item filtered from the stream.&lt;br/&gt; This is a hash for a merkle tree node where the contents of that part of the merkle tree have been removed from this stream. &lt;p&gt; Items of this type SHALL NOT be present in the full (unfiltered) block stream.&lt;br/&gt; Items of this type SHALL replace any item removed from a partial (filtered) block stream.&lt;br/&gt; Presence of `filtered_item` entries SHALL NOT prevent verification of a block, but MAY preclude verification or reconstruction of consensus state.&lt;br/&gt; |
| block_proof | [BlockProof](#com-hedera-hapi-block-stream-BlockProof) |  | A signed block proof.&lt;br/&gt; The signed merkle proof for this block. This will validate a &#34;virtual&#34; merkle tree containing the previous block &#34;virtual&#34; root, an &#34;input&#34; subtree, an &#34;output&#34; subtree, and a &#34;state changes&#34; subtree. &lt;p&gt; This item is not part of the block stream hash chain/tree, and MUST follow after the end of a block. |
| record_file | [RecordFileItem](#com-hedera-hapi-block-stream-RecordFileItem) |  | A record file and associated data. &lt;p&gt; This MUST contain a single Record file, associated Sidecar files, and data from related Signature files. If this item is present, special treatment is REQUIRED for this block. &lt;ul&gt; &lt;li&gt;The block SHALL NOT have a `BlockHeader`.&lt;/li&gt; &lt;li&gt;The block SHALL NOT have a `BlockProof`.&lt;/li&gt; &lt;li&gt;The block SHALL contain _exactly one_ `RecordFileItem`.&lt;/li&gt; &lt;li&gt;The block SHALL NOT contain any item other than a `RecordFileItem`.&lt;/li&gt; &lt;li&gt;The content of the `RecordFileItem` MUST be validated using the signature data and content provided within according to the process used for Record Files prior to the creation of Block Stream.&lt;/li&gt; &lt;/ul&gt; |






<a name="com-hedera-hapi-block-stream-FilteredItemHash"></a>

### FilteredItemHash
Verification data for an item filtered from the stream.

Items of this type SHALL NOT be present in the full (unfiltered) block
stream.&lt;br/&gt;
Items of this type SHALL replace any item removed from a partial (filtered)
block stream.&lt;br/&gt;
Presence of `filtered_item` entries SHALL NOT prevent verification
of a block, but MAY preclude verification or reconstruction
of consensus state.&lt;br/&gt;


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| item_hash | [bytes](#bytes) |  | A hash of an item filtered from the stream. &lt;p&gt; The hash algorithm used MUST match the hash algorithm specified in the block header for the containing block.&lt;br/&gt; This field is REQUIRED. |
| filtered_path | [uint64](#uint64) |  | A record of the merkle path to the item that was filtered from the stream.&lt;br/&gt; This path begins at the root of the block proof merkle tree. &lt;p&gt; This REQUIRED field SHALL describe the full path in the virtual merkle tree constructed for the block proof that contained the item filtered from the stream. |





 

 

 

 



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

