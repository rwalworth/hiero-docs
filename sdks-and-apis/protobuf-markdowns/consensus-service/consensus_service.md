# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/output/consensus_service.proto](#block_stream_output_consensus_service-proto)
    - [CreateTopicOutput](#com-hedera-hapi-block-stream-output-CreateTopicOutput)
    - [DeleteTopicOutput](#com-hedera-hapi-block-stream-output-DeleteTopicOutput)
    - [SubmitMessageOutput](#com-hedera-hapi-block-stream-output-SubmitMessageOutput)
    - [UpdateTopicOutput](#com-hedera-hapi-block-stream-output-UpdateTopicOutput)
  
    - [RunningHashVersion](#com-hedera-hapi-block-stream-output-RunningHashVersion)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_output_consensus_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/output/consensus_service.proto
# Consensus Service
Block stream messages that report the results of transactions handled
by the `Consensus` service.

### Topic Running Hash Calculation
Submitted messages include a topic running hash. This value has changed
over time, with the known versions detailed in the `RunningHashVersion`
enumeration. The running hash version SHALL NOT be transmitted in the
Block Stream, however, as it is a fixed value that can only be changed
with a new release of consensus node software following a HIP to update
the specification, so repeating that fixed value in the block stream is
wasteful and unnecessary.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-output-CreateTopicOutput"></a>

### CreateTopicOutput
Block Stream data for a `createTopic` transaction.

This message SHALL NOT duplicate information already contained in the
original transaction.






<a name="com-hedera-hapi-block-stream-output-DeleteTopicOutput"></a>

### DeleteTopicOutput
Block Stream data for a `deleteTopic` transaction.

This message SHALL NOT duplicate information already contained in the
original transaction.






<a name="com-hedera-hapi-block-stream-output-SubmitMessageOutput"></a>

### SubmitMessageOutput
Block Stream data for a `submitMessage` transaction.

This message SHALL NOT duplicate information already contained in the
original transaction.&lt;br/&gt;
The actual topic running hash SHALL be present in a `StateChanges` block
item, and is not duplicated here.






<a name="com-hedera-hapi-block-stream-output-UpdateTopicOutput"></a>

### UpdateTopicOutput
Block Stream data for a `updateTopic` transaction.

This message SHALL NOT duplicate information already contained in the
original transaction.





 


<a name="com-hedera-hapi-block-stream-output-RunningHashVersion"></a>

### RunningHashVersion
A version of the topic running hash.

The inputs to the topic running hash have changed over time.
This is tracked in earlier record streams as an integer. For the
block stream we chose to use an enumeration for both efficiency
and clarity. Placing the most recent, and most common/highest
volume, version as `0` reduces the serialized size of this message
by not serializing that default value.

&lt;hr style=&#34;margin: 0.2em 5em 0.2em 5em; height: 0.5em;
    border-style: solid none solid none; border-width: 2px;&#34;/&gt;

The topic running hash is a 48-byte value that is the output
of a hash digest with input data determined by the value of
the `topic_running_hash_version` field.&lt;br/&gt;
All new transactions SHALL use `topic_running_hash_version`
`WITH_MESSAGE_DIGEST_AND_PAYER`.&lt;br/&gt;

| Name | Number | Description |
| ---- | ------ | ----------- |
| WITH_MESSAGE_DIGEST_AND_PAYER | 0 | The most recent version. &lt;p&gt; This version SHALL include, in order &lt;ol&gt; &lt;li&gt;The previous running hash of the topic (48 bytes)&lt;/li&gt; &lt;li&gt;The `topic_running_hash_version` field (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The number of seconds since the epoch when the `ConsensusSubmitMessage` reached consensus (8 bytes)&lt;/li&gt; &lt;li&gt;The number of nanoseconds within the second when the `ConsensusSubmitMessage` reached consensus (4 bytes)&lt;/li&gt; &lt;li&gt;The `topic_sequence_number` field (8 bytes)&lt;/li&gt; &lt;li&gt;The output of a SHA-384 digest of the message bytes from the `ConsensusSubmitMessage` (48 bytes)&lt;/li&gt; &lt;/ol&gt; |
| WITH_MESSAGE_DIGEST | 1 | An earlier version. &lt;p&gt; This version SHALL include, in order &lt;ol&gt; &lt;li&gt;The previous running hash of the topic (48 bytes)&lt;/li&gt; &lt;li&gt;The `topic_running_hash_version` field (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The number of seconds since the epoch when the `ConsensusSubmitMessage` reached consensus (8 bytes)&lt;/li&gt; &lt;li&gt;The number of nanoseconds within the second when the `ConsensusSubmitMessage` reached consensus (4 bytes)&lt;/li&gt; &lt;li&gt;The `topic_sequence_number` field (8 bytes)&lt;/li&gt; &lt;li&gt;The output of a SHA-384 digest of the message bytes from the `ConsensusSubmitMessage` (48 bytes)&lt;/li&gt; &lt;/ol&gt; |
| WITH_FULL_MESSAGE | 2 | The original version, used at genesis. &lt;p&gt; This version SHALL include, in order &lt;ol&gt; &lt;li&gt;The previous running hash of the topic (48 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The number of seconds since the epoch when the `ConsensusSubmitMessage` reached consensus (8 bytes)&lt;/li&gt; &lt;li&gt;The number of nanoseconds within the second when the `ConsensusSubmitMessage` reached consensus (4 bytes)&lt;/li&gt; &lt;li&gt;The `topic_sequence_number` field (8 bytes)&lt;/li&gt; &lt;li&gt;The message bytes from the `ConsensusSubmitMessage` (variable)&lt;/li&gt; &lt;/ol&gt; |


 

 

 



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

