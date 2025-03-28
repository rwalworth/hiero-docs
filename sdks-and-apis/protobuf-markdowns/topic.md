# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/consensus/topic.proto](#services_state_consensus_topic-proto)
    - [Topic](#proto-Topic)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_consensus_topic-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/consensus/topic.proto
# HCS Topic Information
The current network state of an Hedera Consensus Service (HCS) Topic.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-Topic"></a>

### Topic
Representation of an Hedera Consensus Service(HCS) topic.

As with all network entities, a topic has a unique entity number, which is
usually given along with the network&#39;s shard and realm in the form of a
shard.realm.number id.&lt;br/&gt;
An HCS topic is an ordered logical stream of messages united and secured by
a running hash of those messages. The integrity of any message on a topic,
and the topic stream as a whole, can always be ascertained from block stream
data by recomputing the running hash across any subset of messages on the
topic.&lt;br/&gt;
The messages on a topic SHALL NOT be stored in network state, but are
available in the network block stream, and may be queried via the
Mirror Node system.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| topic_id | [TopicID](#proto-TopicID) |  | The topic&#39;s ID. &lt;p&gt; This value SHALL be unique within the network. |
| sequence_number | [int64](#int64) |  | The number of messages sent to the topic. |
| expiration_second | [int64](#int64) |  | The expiration time for this topic, in seconds since the epoch. &lt;p&gt; For this purpose, `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`. |
| auto_renew_period | [int64](#int64) |  | The number of seconds for which the topic will be automatically renewed upon expiring (if it has a valid auto-renew account). |
| auto_renew_account_id | [AccountID](#proto-AccountID) |  | The id of the account (if any) that the network will attempt to charge fees to complete auto-renewal of this topic, upon expiration. |
| deleted | [bool](#bool) |  | A flag indicating that this topic is deleted. |
| running_hash | [bytes](#bytes) |  | The current running hash of this topic. &lt;p&gt; This 48-byte field is the output of a SHA-384 digest with input data determined by the current version of the running hash algorithm used by the network.&lt;br/&gt; All topics in state SHALL use running hash algorithm version `3`.&lt;br/&gt; The bytes of each uint64 or uint32 encoded for the hash input MUST be in Big-Endian format. &lt;p&gt; &lt;hr/&gt; If the algorithm version is &#39;3&#39;, then the input data to the SHA-384 digest are, in order: &lt;ol&gt; &lt;li&gt;The previous running hash of the topic (48 bytes)&lt;/li&gt; &lt;li&gt;The `topicRunningHashVersion` (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The number of seconds since the epoch when the `ConsensusSubmitMessage` reached consensus (8 bytes)&lt;/li&gt; &lt;li&gt;The number of nanoseconds within the second when the `ConsensusSubmitMessage` reached consensus (4 bytes)&lt;/li&gt; &lt;li&gt;The `topicSequenceNumber` (8 bytes)&lt;/li&gt; &lt;li&gt;The output of a SHA-384 digest of the message bytes from the `ConsensusSubmitMessage` (48 bytes)&lt;/li&gt; &lt;/ol&gt; &lt;blockquote&gt;Note that older messages on a topic, which are available in the block stream, MAY use older algorithm versions, and the block stream record incorporates a running hash version field to ensure the correct hash calculation for each such historical message.&lt;/blockquote&gt; |
| memo | [string](#string) |  | A short description of this topic. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| admin_key | [Key](#proto-Key) |  | Access control for modification of the topic. &lt;p&gt; If this field is set, that key MUST sign each message to update or delete this topic.&lt;br/&gt; A topic without an admin key SHALL be immutable, except for expiration and renewal.&lt;br/&gt; If this field is not set, the `auto_renew_account_id` MUST NOT be set. |
| submit_key | [Key](#proto-Key) |  | Access control for message submission to the topic. &lt;p&gt; If this field is set, that key MUST sign each consensus submit message for this topic. |
| fee_schedule_key | [Key](#proto-Key) |  | Access control for update/delete of custom fees. &lt;p&gt; If this field is unset, the current custom fees CANNOT be changed.&lt;br/&gt; If this field is set, that `Key` MUST sign any transaction to update the custom fee schedule for this topic. |
| fee_exempt_key_list | [Key](#proto-Key) | repeated | A set of &#34;privileged payer&#34; keys&lt;br/&gt; Keys in this list are permitted to submit messages to this topic without paying custom fees associated with this topic. &lt;p&gt; If a submit transaction is signed by _any_ key included in this set, custom fees SHALL NOT be charged for that transaction.&lt;br/&gt; A `fee_exempt_key_list` MUST NOT contain more than `MAX_ENTRIES_FOR_FEE_EXEMPT_KEY_LIST` keys.&lt;br/&gt; A `fee_exempt_key_list` MUST NOT contain any duplicate keys.&lt;br/&gt; A `fee_exempt_key_list` MAY contain keys for accounts that are inactive, deleted, or non-existent. If not set, there SHALL NOT be any fee-exempt keys. In particular, the following keys SHALL NOT be implicitly or automatically added to this list: `adminKey`, `submitKey`, `fee_schedule_key`. |
| custom_fees | [FixedCustomFee](#proto-FixedCustomFee) | repeated | A set of custom fee definitions.&lt;br/&gt; These are fees to be assessed for each submit to this topic. &lt;p&gt; If this list is empty, the only fees charged for a submit to this topic SHALL be the network and node fees.&lt;br/&gt; If this list is not empty, each fee defined in this set SHALL be evaluated for each message submitted to this topic, and the resultant total assessed fees SHALL be charged.&lt;br/&gt; If this list is not empty, custom fees defined here SHALL be charged _in addition to_ the base network and node fees. |





 

 

 

 



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

