# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/consensus_submit_message.proto](#services_consensus_submit_message-proto)
    - [ConsensusMessageChunkInfo](#proto-ConsensusMessageChunkInfo)
    - [ConsensusSubmitMessageTransactionBody](#proto-ConsensusSubmitMessageTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_consensus_submit_message-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/consensus_submit_message.proto
# Submit Message
Submit a message to a topic via the Hedera Consensus Service (HCS).

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ConsensusMessageChunkInfo"></a>

### ConsensusMessageChunkInfo
Consensus message &#34;chunk&#34; detail.&lt;br/&gt;
This message carries information describing the way in which a message
submitted for consensus is broken into multiple fragments to fit within
network transaction size limits.

The use of multiple message fragments is RECOMMENDED for any message
greater than 4KiB in total size.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| initialTransactionID | [TransactionID](#proto-TransactionID) |  | The TransactionID of the first chunk. &lt;p&gt; This MUST be set for every chunk in a fragmented message. |
| total | [int32](#int32) |  | The total number of chunks in the message. |
| number | [int32](#int32) |  | The sequence number (from 1 to total) of the current chunk in the message. |






<a name="proto-ConsensusSubmitMessageTransactionBody"></a>

### ConsensusSubmitMessageTransactionBody
Submit a message for consensus.&lt;br/&gt;
This transaction adds a new entry to the &#34;end&#34; of a topic, and provides
the core function of the consensus service.

Valid and authorized messages on valid topics SHALL be ordered by the
consensus service, published in the block stream, and available to all
subscribers on this topic via the mirror nodes.&lt;br/&gt;
If this transaction succeeds the resulting `TransactionReceipt` SHALL contain
the latest `topicSequenceNumber` and `topicRunningHash` for the topic.&lt;br/&gt;
If the topic `submitKey` is set, and not an empty `KeyList`, then that key
MUST sign this transaction.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| topicID | [TopicID](#proto-TopicID) |  | Topic to submit message to. |
| message | [bytes](#bytes) |  | A message to be submitted. &lt;p&gt; This Transaction (including signatures) MUST be less than 6KiB.&lt;br/&gt; Messages SHOULD be less than 4KiB. A &#34;chunked&#34; message MAY be submitted if a message larger than this is required. |
| chunkInfo | [ConsensusMessageChunkInfo](#proto-ConsensusMessageChunkInfo) |  | Information for the current &#34;chunk&#34; in a fragmented message. &lt;p&gt; This value is REQUIRED if the full `message` is submitted in two or more fragments due to transaction size limits.&lt;br/&gt; If the message is submitted in a single transaction, then this field SHOULD NOT be set. |





 

 

 

 



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

