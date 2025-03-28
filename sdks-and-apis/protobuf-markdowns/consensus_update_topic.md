# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/consensus_update_topic.proto](#services_consensus_update_topic-proto)
    - [ConsensusUpdateTopicTransactionBody](#proto-ConsensusUpdateTopicTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_consensus_update_topic-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/consensus_update_topic.proto
# Update Topic
Update a topic for the Hedera Consensus Service (HCS).

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ConsensusUpdateTopicTransactionBody"></a>

### ConsensusUpdateTopicTransactionBody
Update the fields of an existing HCS topic.

The topicID field is REQUIRED. All other fields are OPTIONAL.&lt;br/&gt;
Fields set on this transaction SHALL be updated.&lt;br/&gt;
Fields _not_ set on this transaction SHALL NOT be updated.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| topicID | [TopicID](#proto-TopicID) |  | The topic ID specifying the topic to update. &lt;p&gt; A topic with this ID MUST exist and MUST NOT be deleted.&lt;br/&gt; This value is REQUIRED. |
| memo | [google.protobuf.StringValue](#google-protobuf-StringValue) |  | An updated memo to be associated with this topic. &lt;p&gt; If this value is set, the current `adminKey` for the topic MUST sign this transaction.&lt;br/&gt; This value, if set, SHALL be encoded UTF-8 and SHALL NOT exceed 100 bytes when so encoded. |
| expirationTime | [Timestamp](#proto-Timestamp) |  | An updated expiration time for this topic, in seconds since the epoch. &lt;p&gt; For this purpose, `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`.&lt;br/&gt; The expirationTime MUST NOT be greater than the sum of `MAX_AUTORENEW_PERIOD` and the actual consensus timestamp of this transaction.&lt;br/&gt; If `adminKey` is &lt;b&gt;unset&lt;/b&gt; for the _topic_, this transaction MUST NOT modify any other field. |
| adminKey | [Key](#proto-Key) |  | Updated access control for modification of the topic. &lt;p&gt; If this field is set, that key and the previously set key MUST both sign this transaction.&lt;br/&gt; If this value is an empty `KeyList`, the prior key MUST sign this transaction, and the topic SHALL be immutable after this transaction completes, except for expiration and renewal. |
| submitKey | [Key](#proto-Key) |  | Updated access control for message submission to the topic. &lt;p&gt; If this value is set, the current `adminKey` for the topic MUST sign this transaction.&lt;br/&gt; If this value is set to an empty `KeyList`, the `submitKey` for the topic will be unset after this transaction completes. When the `submitKey` is unset, any account may submit a message on the topic, without restriction. |
| autoRenewPeriod | [Duration](#proto-Duration) |  | An updated value for the number of seconds by which the topic expiration will be automatically extended upon expiration, if it has a valid auto-renew account. &lt;p&gt; If this value is set, the current `adminKey` for the topic MUST sign this transaction.&lt;br/&gt; This value, if set, MUST be greater than the configured MIN_AUTORENEW_PERIOD.&lt;br/&gt; This value, if set, MUST be less than the configured MAX_AUTORENEW_PERIOD. |
| autoRenewAccount | [AccountID](#proto-AccountID) |  | An updated ID for the account to be charged renewal fees at the topic&#39;s `expirationTime` to extend the lifetime of the topic. &lt;p&gt; If this value is set and not the &#34;sentinel account&#34;, the referenced account MUST sign this transaction.&lt;br/&gt; If this value is set, the current `adminKey` for the topic MUST sign this transaction.&lt;br/&gt; If this value is set to the &#34;sentinel account&#34;, which is `0.0.0`, the `autoRenewAccount` SHALL be removed from the topic. |
| fee_schedule_key | [Key](#proto-Key) |  | Access control for update/delete of custom fees. &lt;p&gt; If set, subsequent consensus_update_topic transactions signed with this key MAY update or delete the custom fees for this topic.&lt;br/&gt; If this field is set, the admin key MUST sign this transaction.&lt;br/&gt; If this field is set, the previous value SHALL be replaced.&lt;br/&gt; If set to a &#39;Key&#39; containing an empty &#39;KeyList&#39;, the previous value SHALL be cleared.&lt;br/&gt; If not set, the current key SHALL NOT change.&lt;br/&gt; If unset in state, this field MUST NOT be set in this transaction.&lt;br/&gt; If not set when the topic is created, this field CANNOT be set via update.&lt;br/&gt; |
| fee_exempt_key_list | [FeeExemptKeyList](#proto-FeeExemptKeyList) |  | A set of keys&lt;br/&gt; Keys in this list are permitted to submit messages to this topic without paying custom fees associated with this topic. &lt;p&gt; If a submit transaction is signed by _any_ key included in this set, custom fees SHALL NOT be charged for that transaction.&lt;br/&gt; If this field is not set, the current set of keys SHALL NOT change.&lt;br/&gt; If this field is set, but contains an empty list, any existing fee-exempt keys SHALL be removed.&lt;br/&gt; A `fee_exempt_key_list` MUST NOT contain more than `MAX_ENTRIES_FOR_FEE_EXEMPT_KEY_LIST` keys.&lt;br/&gt; A `fee_exempt_key_list` MUST NOT contain any duplicate keys.&lt;br/&gt; A `fee_exempt_key_list` MAY contain keys for accounts that are inactive, deleted, or non-existent. |
| custom_fees | [FixedCustomFeeList](#proto-FixedCustomFeeList) |  | A set of custom fee definitions.&lt;br/&gt; These are fees to be assessed for each submit to this topic. &lt;p&gt; Each fee defined in this set SHALL be evaluated for each message submitted to this topic, and the resultant total assessed fees SHALL be charged.&lt;br/&gt; Custom fees defined here SHALL be assessed in addition to the base network and node fees.&lt;br/&gt; If this field is not set, the current set of custom fees SHALL NOT change.&lt;br/&gt; If this field is set, but contains an empty list, all current custom fees SHALL be removed. custom_fees list SHALL NOT contain more than `MAX_CUSTOM_FEE_ENTRIES_FOR_TOPICS` entries. |





 

 

 

 



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

