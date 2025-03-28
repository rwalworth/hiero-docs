# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/consensus_topic_info.proto](#services_consensus_topic_info-proto)
    - [ConsensusTopicInfo](#proto-ConsensusTopicInfo)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_consensus_topic_info-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/consensus_topic_info.proto
# Topic Information
Query response describing a topic of the Hedera Consensus Service (HCS).

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ConsensusTopicInfo"></a>

### ConsensusTopicInfo
A query response describing the current state of a topic for the Hedera
Consensus Service (HCS).


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| memo | [string](#string) |  | A short description of this topic. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| runningHash | [bytes](#bytes) |  | The latest running hash of the topic. &lt;p&gt; This 48-byte field is the output of a SHA-384 digest with input data determined by the current version of the running hash algorithm used by the network.&lt;br/&gt; All new transactions SHALL use algorithm version `3`.&lt;br/&gt; The bytes of each uint64 or uint32 encoded for the hash input MUST be in Big-Endian format. &lt;p&gt; &lt;hr/&gt; If the algorithm version is &#39;3&#39;, then the input data to the SHA-384 digest are, in order: &lt;ol&gt; &lt;li&gt;The previous running hash of the topic (48 bytes)&lt;/li&gt; &lt;li&gt;The `topicRunningHashVersion` (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The number of seconds since the epoch when the `ConsensusSubmitMessage` reached consensus (8 bytes)&lt;/li&gt; &lt;li&gt;The number of nanoseconds within the second when the `ConsensusSubmitMessage` reached consensus (4 bytes)&lt;/li&gt; &lt;li&gt;The `topicSequenceNumber` (8 bytes)&lt;/li&gt; &lt;li&gt;The output of a SHA-384 digest of the message bytes from the `ConsensusSubmitMessage` (48 bytes)&lt;/li&gt; &lt;/ol&gt; |
| sequenceNumber | [uint64](#uint64) |  | A current sequence number (starting at 1 for the first message) for messages on this topic. |
| expirationTime | [Timestamp](#proto-Timestamp) |  | An expiration time for this topic, in seconds since the epoch. &lt;p&gt; For this purpose, `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`. |
| adminKey | [Key](#proto-Key) |  | A key that MUST sign any transaction to update or delete this topic. &lt;p&gt; If this value is not set (null) then the topic CANNOT be deleted, modified, or updated. |
| submitKey | [Key](#proto-Key) |  | A key that MUST sign any transaction to submit a message to this topic. &lt;p&gt; If this value is not set (null) then any account MAY submit messages to this topic. |
| autoRenewPeriod | [Duration](#proto-Duration) |  | A duration, in seconds, to extend the `expirationTime` value when this topic is automatically renewed. &lt;p&gt; If the `autoRenewAccount` value for this topic is set to a valid account with sufficient HBAR balance to pay renewal fees when this topic expires, the system SHALL automatically renew this topic, extending the `expirationTime` value by the number of seconds described here.&lt;br/&gt; If, however, the `autoRenewAccount` lacks sufficient HBAR balance to pay renewal fees when this topic expires, this topic SHALL be deleted after the time period specified in the `AUTORENEW_GRACE_PERIOD` configuration value. |
| autoRenewAccount | [AccountID](#proto-AccountID) |  | An account that is designated to pay automatic renewal fees. &lt;p&gt; If this value is a valid account ID when this topic expires, this account SHALL be charged the renewal fees for this topic, if it holds sufficient HBAR balance. If the account does not hold sufficient HBAR balance to pay renewal fees when necessary, then this topic SHALL be deleted.&lt;br/&gt; If this value is not set (null), or is not a valid account ID, when this topic expires, then this topic SHALL be deleted after the time period specified in the `AUTORENEW_GRACE_PERIOD` configuration value. |
| ledger_id | [bytes](#bytes) |  | A ledger ID of the network that generated this response. &lt;p&gt; This value SHALL identify the distributed ledger that responded to this query. |
| fee_schedule_key | [Key](#proto-Key) |  | Access control for update/delete of custom fees. &lt;p&gt; If unset, custom fees CANNOT be set for this topic.&lt;br/&gt; If not set when the topic is created, this field CANNOT be set via update.&lt;br/&gt; If set when the topic is created, this field MAY be changed via update. |
| fee_exempt_key_list | [Key](#proto-Key) | repeated | A set of keys.&lt;br/&gt; Keys in this list are permitted to submit messages to this topic without paying custom fees associated with this topic. &lt;p&gt; If a topic submit message is signed by _any_ key included in this set, custom fees SHALL NOT be charged for that transaction.&lt;br/&gt; `fee_exempt_key_list` MAY contain keys for accounts that are inactive, deleted, or non-existent.&lt;br/&gt; If not set, there SHALL NOT be any fee-exempt keys. In particular, the following keys SHALL NOT be implicitly or automatically added to this list: `adminKey`, `submitKey`, `fee_schedule_key`. A `fee_exempt_key_list` MUST NOT contain more than `MAX_ENTRIES_FOR_FEE_EXEMPT_KEY_LIST` keys. A `fee_exempt_key_list` MUST NOT contain any duplicate keys. |
| custom_fees | [FixedCustomFee](#proto-FixedCustomFee) | repeated | A set of custom fee definitions.&lt;br/&gt; These are fees to be assessed for each submit to this topic. &lt;p&gt; Each fee defined in this set SHALL be evaluated for each message submitted to this topic, and the resultant total assessed fees SHALL be charged.&lt;br/&gt; Custom fees defined here SHALL be assessed in addition to the base network and node fees. |





 

 

 

 



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

