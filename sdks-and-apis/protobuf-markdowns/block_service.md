# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/block_service.proto](#block_block_service-proto)
    - [BlockItemSet](#com-hedera-hapi-block-BlockItemSet)
    - [BlockNodeVersions](#com-hedera-hapi-block-BlockNodeVersions)
    - [PublishStreamRequest](#com-hedera-hapi-block-PublishStreamRequest)
    - [PublishStreamResponse](#com-hedera-hapi-block-PublishStreamResponse)
    - [PublishStreamResponse.Acknowledgement](#com-hedera-hapi-block-PublishStreamResponse-Acknowledgement)
    - [PublishStreamResponse.BlockAcknowledgement](#com-hedera-hapi-block-PublishStreamResponse-BlockAcknowledgement)
    - [PublishStreamResponse.EndOfStream](#com-hedera-hapi-block-PublishStreamResponse-EndOfStream)
    - [PublishStreamResponse.ResendBlock](#com-hedera-hapi-block-PublishStreamResponse-ResendBlock)
    - [PublishStreamResponse.SkipBlock](#com-hedera-hapi-block-PublishStreamResponse-SkipBlock)
    - [ServerStatusRequest](#com-hedera-hapi-block-ServerStatusRequest)
    - [ServerStatusResponse](#com-hedera-hapi-block-ServerStatusResponse)
    - [SingleBlockRequest](#com-hedera-hapi-block-SingleBlockRequest)
    - [SingleBlockResponse](#com-hedera-hapi-block-SingleBlockResponse)
    - [StateSnapshotRequest](#com-hedera-hapi-block-StateSnapshotRequest)
    - [StateSnapshotResponse](#com-hedera-hapi-block-StateSnapshotResponse)
    - [SubscribeStreamRequest](#com-hedera-hapi-block-SubscribeStreamRequest)
    - [SubscribeStreamResponse](#com-hedera-hapi-block-SubscribeStreamResponse)
  
    - [PublishStreamEndCode](#com-hedera-hapi-block-PublishStreamEndCode)
    - [PublishStreamResponseCode](#com-hedera-hapi-block-PublishStreamResponseCode)
    - [SingleBlockResponseCode](#com-hedera-hapi-block-SingleBlockResponseCode)
    - [StateSnapshotResponseCode](#com-hedera-hapi-block-StateSnapshotResponseCode)
    - [SubscribeStreamResponseCode](#com-hedera-hapi-block-SubscribeStreamResponseCode)
  
    - [BlockAccessService](#com-hedera-hapi-block-BlockAccessService)
    - [BlockNodeService](#com-hedera-hapi-block-BlockNodeService)
    - [BlockStreamService](#com-hedera-hapi-block-BlockStreamService)
    - [StateService](#com-hedera-hapi-block-StateService)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_block_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/block_service.proto
# Block Service
The Service API exposed by the Block Nodes.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-BlockItemSet"></a>

### BlockItemSet
A wrapper around a repeated BlockItem.&lt;br/&gt;
This message is required so that we can include ordered lists of block
items as `oneof` alternatives in streams.

Each `BlockItemSet` MUST contain at least one `BlockItem`,
and MAY contain up to one full block.&lt;br/&gt;
A single `BlockItemSet` SHALL NOT contain block items from
more than one block.&lt;br/&gt;
If a `BlockHeader` is present in a `BlockItemSet`, that item
MUST be the first item in the list.&lt;br/&gt;
If a `BlockProof` is present in a `BlockItemSet`, that item
MUST be the last item in the list.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block_items | [stream.BlockItem](#com-hedera-hapi-block-stream-BlockItem) | repeated | An ordered list of `BlockItem`s.&lt;br/&gt; This list supports sending block items to subscribers in batches for greater channel efficiency. |






<a name="com-hedera-hapi-block-BlockNodeVersions"></a>

### BlockNodeVersions
Version information for a Block-Node.

The `stream_proto_version` SHOULD be an officially released Block Stream
version.
The `address_book_version` SHALL be determined by networks of Block-Nodes.
The `software_version` SHALL be determined by the implementation of this
specification.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| address_book_version | [proto.SemanticVersion](#proto-SemanticVersion) |  | A version of the Block-Node network address book.&lt;br/&gt; The address book version describes what version of address book this Block-Node holds for discovering and identifying other Block-Nodes. &lt;p&gt; This version SHALL be specific to each &#34;network&#34; of interconnected Block-Nodes. |
| stream_proto_version | [proto.SemanticVersion](#proto-SemanticVersion) |  | A version of the Block Stream specification.&lt;br/&gt; This is the Stream version currently supported by this Block-Node. &lt;p&gt; Implementations SHOULD make reasonable effort to ensure the latest released Block Stream version is supported.&lt;br/&gt; This version MUST be an officially released Block Stream version if the responding Block-Node is not private. |
| software_version | [proto.SemanticVersion](#proto-SemanticVersion) |  | A version of the Block-Node software.&lt;br/&gt; This is the software version that this Block-Node is currently running. &lt;p&gt; This value is implementation-defined. |






<a name="com-hedera-hapi-block-PublishStreamRequest"></a>

### PublishStreamRequest
Publish a stream of block items.

Each request in the stream MUST contain at least one `BlockItem`.&lt;br/&gt;
Each request MAY contain more than one `BlockItem`.&lt;br/&gt;
A single request MUST NOT contain `BlockItem`s from more than one block.&lt;br/&gt;
Each request MAY contain a variable number of `BlockItem`s.&lt;br/&gt;
Each Block MUST begin with a single `BlockHeader` block item.&lt;br/&gt;
If a `BlockHeader` is present in a request, it MUST be the first `BlockItem`
in the `block_items` list.&lt;br/&gt;
The Block-Node SHALL append each `BlockItem` to an internal structure
to reconstruct full blocks.&lt;br/&gt;
The Block-Node MUST verify the block proof for each block before sending a
response message acknowledging that block.&lt;br/&gt;
Each Block MUST end with a single `BlockProof` block item.&lt;br/&gt;
If a `BlockProof` is present in a request, it MUST be the last `BlockItem`
in the `block_items` list.&lt;br/&gt;
The Block-Node MUST verify each Block using the `BlockProof` to
ensure all data was received and processed correctly.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block_items | [BlockItemSet](#com-hedera-hapi-block-BlockItemSet) |  | A stream item containing one or more `BlockItem`s. &lt;p&gt; The full stream SHALL consist of many `block_items` messages followed by a single `status` message. |






<a name="com-hedera-hapi-block-PublishStreamResponse"></a>

### PublishStreamResponse
A response to writing a block stream.

This message is sent in response to each Block in a block stream sent
to a Block-Node. The block stream is sent as a stream of messages, and each
message MAY be acknowledged with a message of this type.&lt;br/&gt;
Each `BlockItem` MAY be acknowledged with an `Acknowledgement`
response. Item acknowledgement is an OPTIONAL feature.&lt;br/&gt;
Each completed block SHALL be acknowledged with an `Acknowledgement`
response. Block acknowledgement is a REQUIRED feature.&lt;br/&gt;
A final response SHALL be sent with an `EndOfStream` status result after
the last block stream item is received, or when the Block-Node must end the
stream for any reason.&lt;br/&gt;
At any time, a Block-Node MAY send a `SkipBlock` response to request that
the Publisher skip the current block and resume with the next. When a
Block-Node sends a `SkipBlock`, that Block-Node SHALL subsequently send
an `Acknowledgement` for the same block, or SHALL send a `ResendBlock`
message.
If a failure is detected (which may include a block or block item that
fails validation) an `EndOfStream` response SHALL be sent with a
relevant error status.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| acknowledgement | [PublishStreamResponse.Acknowledgement](#com-hedera-hapi-block-PublishStreamResponse-Acknowledgement) |  | A response sent to acknowledge successful receipt of a block. |
| end_stream | [PublishStreamResponse.EndOfStream](#com-hedera-hapi-block-PublishStreamResponse-EndOfStream) |  | A response sent to request the Publisher end the current stream. |
| skip_block | [PublishStreamResponse.SkipBlock](#com-hedera-hapi-block-PublishStreamResponse-SkipBlock) |  | A response sent to request the Publisher skip the current block. |
| resend_block | [PublishStreamResponse.ResendBlock](#com-hedera-hapi-block-PublishStreamResponse-ResendBlock) |  | A response sent to request the Publisher resend a skipped block. |






<a name="com-hedera-hapi-block-PublishStreamResponse-Acknowledgement"></a>

### PublishStreamResponse.Acknowledgement
A response to acknowledge receipt and verification of a single item
or full block.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block_ack | [PublishStreamResponse.BlockAcknowledgement](#com-hedera-hapi-block-PublishStreamResponse-BlockAcknowledgement) |  | A response type to acknowledge a full and complete block. &lt;p&gt; All Block-Nodes SHOULD acknowledge each block. |






<a name="com-hedera-hapi-block-PublishStreamResponse-BlockAcknowledgement"></a>

### PublishStreamResponse.BlockAcknowledgement
Acknowledgement of a full block.&lt;br/&gt;
This message is a necessary part of the block streaming protocol.

This response SHALL be sent after a block state proof item is
received and verified.&lt;br/&gt;
The Block-Node SHALL send exactly one `BlockAcknowledgement` for
each successful block.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block_number | [uint64](#uint64) |  | A block number number of the acknowledged block. &lt;p&gt; A Publisher SHOULD verify that this value matches the block sent. |
| block_root_hash | [bytes](#bytes) |  | A hash of the virtual merkle root for the block. &lt;p&gt; This SHALL be the hash calculated by the Block-Node for the root node of the virtual merkle tree that is signed by the source system to validate the block. |
| block_already_exists | [bool](#bool) |  | A flag indicating that the received block duplicates an existing block. &lt;p&gt; If a Publisher receives acknowledgement with this flag set true the Publisher MAY end the stream.&lt;br/&gt; The `block_number` returned SHALL be the last block known and verified by the Block-Node.&lt;br/&gt; The Publisher MAY resume publishing immediately after the indicated block. |






<a name="com-hedera-hapi-block-PublishStreamResponse-EndOfStream"></a>

### PublishStreamResponse.EndOfStream
A message sent to end a stream.

This response message SHALL be sent from a Block-Node to a block
stream Publisher when a `publishBlockStream` stream ends.&lt;br/&gt;
This message SHALL be sent exactly once for each `publishBlockStream`
call.&lt;br/&gt;
The Publisher SHALL cease sending block items upon receiving
this response, and MAY determine the ending state of the stream from
the `status` enumeration and the `block_number` returned.&lt;br/&gt;
A Publisher SHOULD verify that the `block_number` value matches the
last block sent, and SHOULD resend one or more blocks if the value
here does not match the expected value.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| status | [PublishStreamResponseCode](#com-hedera-hapi-block-PublishStreamResponseCode) |  | A response code. &lt;p&gt; This code indicates the reason the stream ended.&lt;br/&gt; This value MUST be set to a non-default value. |
| block_number | [uint64](#uint64) |  | The number of the last completed and _verified_ block. &lt;p&gt; Block-Nodes SHOULD only end a stream after a block state proof to avoid the need to resend items.&lt;br/&gt; If status is a failure code, the Publisher MUST start a new stream at the beginning of the first block _following_ this number (e.g. if this is 91827362983, then the new stream must start with the _header_ for block 91827362984). |






<a name="com-hedera-hapi-block-PublishStreamResponse-ResendBlock"></a>

### PublishStreamResponse.ResendBlock
Message indicating the Publisher must resend from a specified block.
A Publisher might resend the block, if it has that block available,
or it might choose to end the stream if it has already received
acknowledgement from a different trustworthy Block-Node.

On receiving this message, a Publisher MUST resume sending blocks at
the block number indicated, or else end the stream.&lt;br/&gt;
This message SHALL only be sent following a `SkipBlock` message for a
block that has not been acknowledged.&lt;br/&gt;
A Block-Node SHALL NOT send a `ResendBlock` message for any block that
is already acknowledged.&lt;br/&gt;
A Publisher MAY choose to end the stream rather than resend the
the requested block.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block_number | [uint64](#uint64) |  | The number of the _unverified_ block to re-send. &lt;p&gt; This SHALL be the block number one greater than the last block this Block-Node has successfully stored and verified. |






<a name="com-hedera-hapi-block-PublishStreamResponse-SkipBlock"></a>

### PublishStreamResponse.SkipBlock
Message indicating the Publisher should skip the current block.

Block-Nodes SHOULD only skip a block if that block is currently being
received from another source.&lt;br/&gt;
Publishers MUST stop sending the current block and resume with
the block header for the next block, or else end the stream.&lt;br/&gt;
A Publisher that receives this message SHALL subsequently receive an
acknowledgement for the skipped block or SHALL receive a `ResendBlock`
message.&lt;br/&gt;
A Publisher asked to skip a block SHOULD NOT delay any subsequent
block, but should send that block as soon as it is available.&lt;br/&gt;
A Publisher MAY be asked to skip multiple blocks in succession, but
SHOULD interpret that as the presence of another Publisher with a
lower latency connection.&lt;br/&gt;
A Publisher MAY choose to end the stream and send the block to a
different Block-Node, rather than skip sending the block to the
current Block-Node.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block_number | [uint64](#uint64) |  | The number of the _unverified_ block to skip. &lt;p&gt; This MUST match the block number of the `BlockHeader` most recently sent by the Publisher. |






<a name="com-hedera-hapi-block-ServerStatusRequest"></a>

### ServerStatusRequest
A request for the status of a Block-Node.






<a name="com-hedera-hapi-block-ServerStatusResponse"></a>

### ServerStatusResponse
A response to a server status request.

This message SHALL provide a client with information needed to successfully
query this Block-Node for a block, stream of blocks, or
state snapshot.&lt;br/&gt;
A request for blocks between `first_available_block` and
`last_available_block`, inclusive, SHOULD succeed. Any request for blocks
outside that range SHOULD fail.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| first_available_block | [uint64](#uint64) |  | The lowest numbered block available on this Block-Node. &lt;p&gt; Any request for a block with lower number than this value SHALL fail with a status value indicating and invalid start block number. |
| last_available_block | [uint64](#uint64) |  | The greatest block number available from this Block-Node. &lt;p&gt; Any request for a block with a block number higher than this value MAY fail. |
| only_latest_state | [bool](#bool) |  | A flag indicating this Block-Node only offers the latest state snapshot. &lt;p&gt; If this value is `true` the client MUST set `retrieve_latest` `true` in any `StateSnapshotRequest` sent to this Block-Node. |
| version_information | [BlockNodeVersions](#com-hedera-hapi-block-BlockNodeVersions) |  | Version information.&lt;br/&gt; Versions for the block network address book, block stream protocol buffer schema, and Block-Node software. |






<a name="com-hedera-hapi-block-SingleBlockRequest"></a>

### SingleBlockRequest
A request to read a single block.

A client system SHALL send this message to request a single block,
including the block state proof.&lt;br/&gt;
A client MAY request that the block be sent without verification.
A compliant Block-Node MAY respond to requests that allow unverified
responses by returning the full requested block before verifying
the included block proof.&lt;br/&gt;
A compliant Block-Node MAY support _only_ requests that allow unverified
blocks, but MUST clearly document that limitation, and MUST respond to
a request that does not allow unverified blocks with the
`ALLOW_UNVERIFIED_REQUIRED` response code.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block_number | [uint64](#uint64) |  | The block number of a block to retrieve. &lt;p&gt; The requested block MUST exist on the Block-Node.&lt;br/&gt; This value MUST NOT be set if `retrieve_latest` is set `true`.&lt;br/&gt; This value MUST be set to a valid block number if `retrieve_latest` is unset or is set `false`. |
| allow_unverified | [bool](#bool) |  | A flag to indicate that the requested block may be sent without verifying its `BlockProof`.&lt;br/&gt; This might be set by a client that expects to perform its own verification and wishes lower latency or, potentially, lower cost. &lt;p&gt; If this value is set, then the responding Block-Node MAY respond with a block that has not completed verification of its `BlockProof`.&lt;br/&gt; If this is _not_ set then the Block-Node MUST respond with either a fully verified and validated block, or `VERIFIED_BLOCK_UNAVAILABLE` if the requested block is not yet verified.&lt;br/&gt; The default value is _not set_. |
| retrieve_latest | [bool](#bool) |  | A flag to request the latest available block. &lt;p&gt; This value MAY be set `true` to request the last block available.&lt;br/&gt; If this value is set to `true` then `block_number` MUST NOT be set and SHALL be ignored. |






<a name="com-hedera-hapi-block-SingleBlockResponse"></a>

### SingleBlockResponse
A response to a `singleBlock` request.

This message SHALL be sent in response to a request, and SHALL contain at
least a valid `status`.&lt;br/&gt;
If `status` is `READ_BLOCK_SUCCESS`, the response SHALL contain the
requested block in the `block` field.

&gt; Note
&gt;&gt; A block can become quite large. A client MUST be prepared to receive the
&gt;&gt; full content of the block, perhaps many megabytes of data.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| status | [SingleBlockResponseCode](#com-hedera-hapi-block-SingleBlockResponseCode) |  | A response status. &lt;p&gt; The reported status SHALL reflect the success of the request, or a detailed reason the request failed. |
| block | [stream.Block](#com-hedera-hapi-block-stream-Block) |  | The requested block. &lt;p&gt; This container object SHALL hold the entire sequence of block items for the requested block.&lt;br/&gt; The block items in this message SHALL be in the same order as received.&lt;br/&gt; The items in this message SHALL begin with a `BlockHeader` and end with a `BlockStateProof` applicable to this block. |






<a name="com-hedera-hapi-block-StateSnapshotRequest"></a>

### StateSnapshotRequest
A request to read a state snapshot.

A state snapshot is a full copy of the network state at the completion of a
particular block.

This request MUST contain a block number that has already reached this
Block-Node and completed verification, or request the &#34;latest&#34; snapshot.&lt;br/&gt;
This request MAY specify the &#34;latest&#34; snapshot, and the Block-Node SHALL
respond with a reference to a snapshot containing the most recent contents
of the network state known to that Block-Node.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| last_block_number | [uint64](#uint64) |  | A block number. &lt;p&gt; This SHALL be the last block number present in the snapshot returned.&lt;br/&gt; If `retrieve_latest` is set `true` this field SHOULD NOT be set and SHALL be ignored.&lt;br/&gt; A Block-Node MAY reject any request with a non-default value for this field, but MUST clearly document that behavior. |
| retrieve_latest | [bool](#bool) |  | A boolean to request the latest available snapshot. &lt;p&gt; This value MAY be set `true` to request the most recent state snapshot available.&lt;br/&gt; If this value is set to `true` then `last_block_number` SHOULD NOT be set and SHALL be ignored.&lt;br/&gt; A Block-Node MAY reject any request with that does _not_ set this field `true`, but MUST clearly document that behavior. |






<a name="com-hedera-hapi-block-StateSnapshotResponse"></a>

### StateSnapshotResponse
A response to a request for a state snapshot.

This message SHALL deliver a _reference_ to the requested snapshot
data if successful.&lt;br/&gt;
This message SHALL deliver a code indicating the reason for failure
if unsuccessful.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| status | [StateSnapshotResponseCode](#com-hedera-hapi-block-StateSnapshotResponseCode) |  | A status response. &lt;p&gt; This code SHALL indicate a successful call, or the detailed reason for failure. |
| last_block_number | [uint64](#uint64) |  | A block number. &lt;p&gt; This SHALL be the number of the last block included in this state snapshot. |
| snapshot_reference | [string](#string) |  | A reference to where the requested state snapshot may be obtained. &lt;p&gt; &lt;blockquote&gt;REVIEW NOTE&lt;blockquote&gt; This is TEMPORARY. We have not yet designed how state snapshots may be sent. One idea is to use `Any` and let implementations decide; another is to use a time limited URL (with the same login as the Block-Node server); another is to use a customer-pays cloud storage bucket. &lt;/blockquote&gt;&lt;/blockquote&gt; |






<a name="com-hedera-hapi-block-SubscribeStreamRequest"></a>

### SubscribeStreamRequest
A request to stream block items from Block-Node to a client.

The Block-Node SHALL respond to this request with a stream of
`SubscribeStreamResponse` messages.&lt;br/&gt;
The Block-Node SHALL stream the full contents of the blocks requested.&lt;br/&gt;
The block items SHALL be streamed in order originally produced within
a block.&lt;br/&gt;
The blocks SHALL be streamed in ascending order by `block_number`.&lt;br/&gt;
The Block-Node SHALL end the stream when the last requested block has
been sent.&lt;br/&gt;
The Block-Node SHALL end the stream with a response code status of SUCCESS
when the stream is complete.&lt;br/&gt;
The client SHOULD call the `serverStatus` rpc prior to constructing this
request to determine the available start and end blocks.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| start_block_number | [uint64](#uint64) |  | A block number to start the stream. &lt;p&gt; This SHALL be the block number of the first block returned.&lt;br/&gt; This field MUST be less than or equal to the latest available block number. |
| end_block_number | [uint64](#uint64) |  | A block number to end the stream.&lt;br/&gt; This is optional, and if not set (0), the stream will be &#34;infinite&#34;. &lt;p&gt; This field MAY be zero (`0`) to indicate the stream SHOULD continue indefinitely, streaming new blocks as each becomes available.&lt;br/&gt; If this value is greater than zero (`0`) &lt;ul&gt; &lt;li&gt;This value SHALL be the number of the last block returned.&lt;/li&gt; &lt;li&gt;This field MUST NOT be less than `start_block_number`.&lt;/li&gt; &lt;li&gt;This SHOULD be a block number that is immediately available from the Block-Node.&lt;/li&gt; &lt;li&gt;A Block-Node SHALL continue to stream blocks until the last requested block is transmitted.&lt;/li&gt; &lt;li&gt;A Block-Node MAY reject a request for a block that is not yet available.&lt;/li&gt; &lt;li&gt;A Block-Node MAY accept future block numbers.&lt;/li&gt; &lt;li&gt;Block-Node implementations MAY charge increased fees for such &#34;future&#34; streams.&lt;/li&gt; &lt;/ul&gt; |
| allow_unverified | [bool](#bool) |  | A flag to indicate that the requested block(s) may be sent before verifying each block&#39;s `BlockProof`.&lt;br/&gt; This might be set by a client that expects to perform its own verification and wishes lower latency or, potentially, lower cost. &lt;p&gt; If this value is set, then the responding Block-Node MAY respond with blocks that have not (yet) completed block proof verification.&lt;br/&gt; If this is _not set_ then the Block-Node MUST respond with only fully verified and validated block(s).&lt;br/&gt; If this is _set_, then a Block-Node MAY stream items from blocks that have not yet been verified or do not yet have a block proof available.&lt;br/&gt; The default value is _not set_. |






<a name="com-hedera-hapi-block-SubscribeStreamResponse"></a>

### SubscribeStreamResponse
One item in a stream of `subscribeBlockStream` responses.

The Block-Node SHALL respond to a `subscribeBlockStream` request with a
stream of `SubscribeStreamResponse` messages.&lt;br/&gt;
The Block-Node SHALL stream the full contents of the blocks requested.&lt;br/&gt;
The block items SHALL be streamed in order originally produced within
a block.&lt;br/&gt;
The blocks SHALL be streamed in ascending order by `block_number`.&lt;br/&gt;
The Block-Node SHALL end the stream when the last requested block has
been sent.&lt;br/&gt;
The Block-Node SHALL end the stream with a response code status of SUCCESS
when the stream is complete.&lt;br/&gt;
The Block-Node SHALL end the stream with a response code status of
`READ_STREAM_INVALID_START_BLOCK_NUMBER` if the start block number is
greater than the end block number.&lt;br/&gt;
The Block-Node SHALL end the stream with a response code status of
`READ_STREAM_INSUFFICIENT_BALANCE` if insufficient balance remains to
complete the requested stream.
The Block-Node SHALL make every reasonable effort to fulfill as much of the
request as available balance supports, in the event balance is not
sufficient to complete the request.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| status | [SubscribeStreamResponseCode](#com-hedera-hapi-block-SubscribeStreamResponseCode) |  | A final response item describing the terminal status of this stream. &lt;p&gt; The Block-Node SHALL end the stream following this message. |
| block_items | [BlockItemSet](#com-hedera-hapi-block-BlockItemSet) |  | A stream response item containing one or more `BlockItem`s. &lt;p&gt; The full stream SHALL consist of many `block_items` messages followed by a single `status` message. |





 


<a name="com-hedera-hapi-block-PublishStreamEndCode"></a>

### PublishStreamEndCode
An enumeration indicating why a Publisher ended a stream.

This enumeration describes the reason a block stream
(sent via `publishBlockStream`) was ended by the Publisher.

| Name | Number | Description |
| ---- | ------ | ----------- |
| STREAM_END_UNKNOWN | 0 | An &#34;unset value&#34; flag, this value SHALL NOT be used.&lt;br/&gt; This status indicates the server software failed to set a status, and SHALL be considered a software defect. |
| STREAM_END_RESET | 1 | The Publisher reached a reset point.&lt;br/&gt; No errors occurred and the source Block-Node orderly ended the stream.

Publishers SHOULD use this code to end a stream and restart occasionally. Occasionally resetting the stream increases stability and allows for routine network configuration changes. |
| STREAM_END_TIMEOUT | 2 | The delay between items was too long.&lt;br/&gt; The destination system did not timely acknowledge a block. &lt;p&gt; The source SHALL start a new stream before the failed block. |
| STREAM_END_ERROR | 3 | The Publisher encountered an error.&lt;br/&gt; The Publisher encountered an internal error and must try again later. &lt;p&gt; Publishers that encounter internal logic errors, find themselves &#34;behind&#34; the network, or otherwise detect an unexpected situation MUST send this code and restart the stream before the failed block. |
| STREAM_END_TOO_FAR_BEHIND | 4 | The Block-Node is too far behind to catch up directly.&lt;br/&gt; The Block-Node responded to a block header with &#34;BEHIND&#34; and is too far behind the Publisher. &lt;p&gt; The Block-Node MUST recover and &#34;catch up&#34; from another trustworthy Block-Node.&lt;br/&gt; The Publisher MAY stream items to a different Block-Node.&lt;br/&gt; The Publisher MAY resume streaming to this Block-Node later.&lt;br/&gt; The `EndOfStream` message MUST include the earliest and latest blocks currently available from the Publisher.&lt;br/&gt; The Block-Node SHOULD attempt to &#34;catch up&#34; to the _latest_ block available from the Publisher. |



<a name="com-hedera-hapi-block-PublishStreamResponseCode"></a>

### PublishStreamResponseCode
An enumeration indicating the status of this request.

This enumeration SHALL describe the reason a block stream
(sent via `publishBlockStream`) ended.

| Name | Number | Description |
| ---- | ------ | ----------- |
| STREAM_ITEMS_UNKNOWN | 0 | An &#34;unset value&#34; flag, this value SHALL NOT be used.&lt;br/&gt; This status indicates the server software failed to set a status, and SHALL be considered a software defect. |
| STREAM_ITEMS_SUCCESS | 1 | The request succeeded.&lt;br/&gt; No errors occurred and the receiving Block-Node orderly ended the stream. &lt;p&gt; The Publisher MAY start a new stream beginning with the next block. |
| STREAM_ITEMS_TIMEOUT | 2 | The delay between items was too long. &lt;p&gt; The source MUST start a new stream before the failed block. |
| STREAM_ITEMS_OUT_OF_ORDER | 3 | An item was received out-of-order.&lt;br/&gt; This might be two headers without a proof between them, or similar. &lt;p&gt; The source MUST start a new stream before the failed block. |
| STREAM_ITEMS_BAD_STATE_PROOF | 4 | A block state proof item could not be validated.&lt;br/&gt; The source MUST start a new stream before the failed block. |
| STREAM_ITEMS_BEHIND | 5 | The Block-Node is &#34;behind&#34; the Publisher.&lt;br/&gt; Ths Publisher has sent a block later than this Block-Node can process. The Publisher may retry by sending blocks immediately following the `block_number` returned, or may connect to another Block-Node. &lt;p&gt; Block-Nodes that are &#34;behind&#34; SHOULD attempt to &#34;catch up&#34; by requesting blocks from another Block-Node or other source of recent historical block stream data. |
| STREAM_ITEMS_INTERNAL_ERROR | 6 | The Block-Node had an internal error and cannot continue processing.&lt;br/&gt; The Publisher may retry again later. |
| STREAM_ITEMS_PERSISTENCE_FAILED | 7 | The Block-Node failed to store the block persistently. &lt;p&gt; The Publisher SHOULD start a new stream to send the block to this Block-Node, or any other reliable Block-Node. The Publisher MUST NOT discard the block until it is successfully persisted and verified (and acknowledged) by at least one Block-Node. |



<a name="com-hedera-hapi-block-SingleBlockResponseCode"></a>

### SingleBlockResponseCode
An enumeration indicating the status of this request.

| Name | Number | Description |
| ---- | ------ | ----------- |
| READ_BLOCK_UNKNOWN | 0 | An &#34;unset value&#34; flag, this value SHALL NOT be used.&lt;br/&gt; This status indicates the server software failed to set a status, and SHALL be considered a software defect. |
| READ_BLOCK_INSUFFICIENT_BALANCE | 1 | The requesting client account lacks sufficient HBAR to pay the service fee for this request.&lt;br/&gt; The client MAY retry the request, but MUST increase the client account balance with this Block-Node before doing so. |
| READ_BLOCK_SUCCESS | 2 | The request succeeded.&lt;br/&gt; The requested block SHALL be returned in the `block` field. |
| READ_BLOCK_NOT_FOUND | 3 | The requested block was not found.&lt;br/&gt; Something failed and a block that should be available was not found. &lt;p&gt; The client MAY retry the request; if this result is repeated the request SHOULD be directed to a different Block-Node. |
| READ_BLOCK_NOT_AVAILABLE | 4 | The requested block is not available on this Block-Node. &lt;p&gt; The client SHOULD send a `serverStatus` request to determine the lowest and highest block numbers available at this Block-Node. |
| ALLOW_UNVERIFIED_REQUIRED | 5 | The request for a verified block cannot be fulfilled.&lt;br/&gt; The client requested a verified block from a Block-Node that does not offer verified blocks. &lt;p&gt; The client MAY retry the request with the `allow_unverified` flag set. |
| VERIFIED_BLOCK_UNAVAILABLE | 6 | The request for a verified block cannot be fulfilled.&lt;br/&gt; The client requested a verified block from a Block-Node but the requested block is not yet verified. &lt;p&gt; The client MAY retry the request after a short delay (typically 2 seconds or more). |



<a name="com-hedera-hapi-block-StateSnapshotResponseCode"></a>

### StateSnapshotResponseCode
An enumeration indicating the status of a StateSnapshotResponse request.

| Name | Number | Description |
| ---- | ------ | ----------- |
| STATE_SNAPSHOT_UNKNOWN | 0 | An &#34;unset value&#34; flag, this value SHALL NOT be used.&lt;br/&gt; This status indicates the server software failed to set a status, and SHALL be considered a software defect. |
| STATE_SNAPSHOT_INSUFFICIENT_BALANCE | 1 | The requesting client account lacks sufficient HBAR to pay the service fee for this request. &lt;p&gt; The client MAY retry the request, but MUST increase the client account balance with this Block-Node before doing so. |
| STATE_SNAPSHOT_SUCCESS | 2 | The request succeeded. &lt;p&gt; The full snapshot data MAY be read via the endpoint provided in the `snapshot_reference` field for the duration specified. |



<a name="com-hedera-hapi-block-SubscribeStreamResponseCode"></a>

### SubscribeStreamResponseCode
An enumeration indicating the status of this request.

This response code SHALL be the last message in the stream of responses.
This code SHALL represent the final status of the full request.

| Name | Number | Description |
| ---- | ------ | ----------- |
| READ_STREAM_UNKNOWN | 0 | An &#34;unset value&#34; flag, this value SHALL NOT be used.&lt;br/&gt; This status indicates the server software failed to set a status, and SHALL be considered a software defect. |
| READ_STREAM_INSUFFICIENT_BALANCE | 1 | The requesting client account lacks sufficient HBAR to pay the service fee for this request. &lt;p&gt; The client MAY retry the request, but MUST increase the client account balance with this Block-Node before doing so. |
| READ_STREAM_SUCCESS | 2 | The request succeeded. &lt;p&gt; The requested block(s) SHALL precede the status response with this value. |
| READ_STREAM_INVALID_START_BLOCK_NUMBER | 3 | The requested start block number is not valid.&lt;br/&gt; The start block number is after the end block number, less than `0`, or otherwise invalid.&lt;br/&gt; The client MAY retry this request, but MUST change the `start_block_number` field to a valid start block. |
| READ_STREAM_INVALID_END_BLOCK_NUMBER | 4 | The requested end block number is not valid.&lt;br/&gt; The end block number is greater than the highest current block number, less than `0`, or otherwise invalid.&lt;br/&gt; The client MAY retry this request, but MUST change the `end_block_number` field to a valid end block. |
| READ_STREAM_NOT_AVAILABLE | 5 | The requested stream is not available.&lt;br/&gt; The client MAY retry again later. |


 

 


<a name="com-hedera-hapi-block-BlockAccessService"></a>

### BlockAccessService
Remote procedure calls (RPCs) for the Block-Node block services.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| singleBlock | [SingleBlockRequest](#com-hedera-hapi-block-SingleBlockRequest) | [SingleBlockResponse](#com-hedera-hapi-block-SingleBlockResponse) | Read a single block from the Block-Node. &lt;p&gt; The request SHALL describe the block number of the block to retrieve. |


<a name="com-hedera-hapi-block-BlockNodeService"></a>

### BlockNodeService
Remote procedure calls (RPCs) for the Block-Node ancillary services.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| serverStatus | [ServerStatusRequest](#com-hedera-hapi-block-ServerStatusRequest) | [ServerStatusResponse](#com-hedera-hapi-block-ServerStatusResponse) | Read the status of this Block-Node. &lt;p&gt; A client SHOULD request server status prior to requesting block stream data or a state snapshot. |


<a name="com-hedera-hapi-block-BlockStreamService"></a>

### BlockStreamService
Remote procedure calls (RPCs) for the Block-Node stream services.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| publishBlockStream | [PublishStreamRequest](#com-hedera-hapi-block-PublishStreamRequest) stream | [PublishStreamResponse](#com-hedera-hapi-block-PublishStreamResponse) stream | Publish a stream of blocks. &lt;p&gt; Each item in the stream MUST contain one `BlockItem`.&lt;br/&gt; Each Block MUST begin with a single `BlockHeader` block item.&lt;br/&gt; The Block-Node SHALL append each `BlockItem` to an internal structure to construct full blocks.&lt;br/&gt; Each Block MUST end with a single `BlockStateProof` block item.&lt;br/&gt; It is RECOMMENDED that the implementations verify the Block using the `BlockStateProof` to validate all data was received correctly.&lt;br/&gt; This API SHOULD, generally, be restricted based on mTLS authentication to a limited set of Publisher (i.e. consensus node) systems. |
| subscribeBlockStream | [SubscribeStreamRequest](#com-hedera-hapi-block-SubscribeStreamRequest) | [SubscribeStreamResponse](#com-hedera-hapi-block-SubscribeStreamResponse) stream | Subscribe to a stream of blocks. &lt;p&gt; Each item in the stream SHALL contain one `BlockItem` or a response code.&lt;br/&gt; The request message MUST specify start and end block numbers to return/&lt;br/&gt; The Block-Node SHALL stream the full contents of the blocks requested.&lt;br/&gt; The block items SHALL be streamed in order originally produced within a block.&lt;br/&gt; The blocks shall be streamed in ascending order by `block_number`.&lt;br/&gt; The Block-Node SHALL end the stream when the last requested block, if set, has been sent.&lt;br/&gt; A request with an end block of `0` SHALL be interpreted to indicate the stream has no end. The Block-Node SHALL continue to stream new blocks as soon as each becomes available.&lt;br/&gt; The Block-Node SHALL end the stream with response code containing a status of SUCCESS when the stream is complete.&lt;br/&gt; The Block-Node SHALL end the stream with a response code containing a status of `READ_STREAM_INVALID_START_BLOCK_NUMBER` if the start block number is greater than the end block number.&lt;br/&gt; The Block-Node SHALL end the stream with a response code containing a status of `READ_STREAM_PAYMENT_INSUFFICIENT` if insufficient payment remains to complete the requested stream.&lt;br/&gt; The Block-Node SHALL make every reasonable effort to fulfill as much of the request as possible in the event payment is not sufficient to complete the request. |


<a name="com-hedera-hapi-block-StateService"></a>

### StateService
Remote procedure calls (RPCs) for the Block-Node state snapshot
and query services.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| stateSnapshot | [StateSnapshotRequest](#com-hedera-hapi-block-StateSnapshotRequest) | [StateSnapshotResponse](#com-hedera-hapi-block-StateSnapshotResponse) | Read a state snapshot from the Block-Node. &lt;p&gt; The request SHALL describe the last block number present in the snapshot.&lt;br/&gt; Block-Node implementations MAY decline a request for a snapshot older than the latest available, but MUST clearly document this behavior. |

 



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

