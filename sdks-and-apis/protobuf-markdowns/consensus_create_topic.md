# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/consensus_create_topic.proto](#services_consensus_create_topic-proto)
    - [ConsensusCreateTopicTransactionBody](#proto-ConsensusCreateTopicTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_consensus_create_topic-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/consensus_create_topic.proto
# Create Topic
Create a new topic for the Hedera Consensus Service (HCS).

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ConsensusCreateTopicTransactionBody"></a>

### ConsensusCreateTopicTransactionBody
Create a topic to accept and group consensus messages.

If `autoRenewAccount` is specified, that account Key MUST also sign this
transaction.&lt;br/&gt;
If `adminKey` is set, that Key MUST sign the transaction.&lt;br/&gt;
On success, the resulting `TransactionReceipt` SHALL contain the newly
created `TopicId`.

The `autoRenewPeriod` on a topic MUST be set to a value between
`autoRenewPeriod.minDuration` and `autoRenewPeriod.maxDuration`. These
values are configurable, typically 30 and 92 days.&lt;br/&gt;
This also sets the initial expirationTime of the topic.

If no `adminKey` is set on a topic
  -`autoRenewAccount` SHALL NOT be set on the topic.
  - A `deleteTopic` transaction SHALL fail.
  - An `updateTopic` transaction that only extends the expirationTime MAY
    succeed.
  - Any other `updateTopic` transaction SHALL fail.

If the topic expires and is not automatically renewed, the topic SHALL enter
the `EXPIRED` state.
  - All transactions on the topic SHALL fail with TOPIC_EXPIRED
     - Except an updateTopic() call that only extends the expirationTime.
  - getTopicInfo() SHALL succeed, and show the topic is expired.
The topic SHALL remain in the `EXPIRED` state for a time determined by the
`autorenew.gracePeriod` (configurable, originally 7 days).&lt;br/&gt;
After the grace period, if the topic&#39;s expirationTime is not extended, the
topic SHALL be automatically deleted from state entirely, and cannot be
recovered or recreated.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| memo | [string](#string) |  | A short memo for this topic. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| adminKey | [Key](#proto-Key) |  | Access control for modification of the topic after it is created. &lt;p&gt; If this field is set, that key MUST sign this transaction.&lt;br/&gt; If this field is set, that key MUST sign each future transaction to update or delete the topic.&lt;br/&gt; An updateTopic transaction that _only_ extends the topic expirationTime (a &#34;manual renewal&#34; transaction) SHALL NOT require admin key signature.&lt;br/&gt; A topic without an admin key SHALL be immutable, except for expiration and renewal.&lt;br/&gt; If adminKey is not set, then `autoRenewAccount` SHALL NOT be set. |
| submitKey | [Key](#proto-Key) |  | Access control for message submission to the topic. &lt;p&gt; If this field is set, that key MUST sign each consensus submit message for this topic.&lt;br/&gt; If this field is not set then any account may submit a message on the topic, without restriction. |
| autoRenewPeriod | [Duration](#proto-Duration) |  | The initial lifetime, in seconds, for the topic.&lt;br/&gt; This is also the number of seconds for which the topic SHALL be automatically renewed upon expiring, if it has a valid auto-renew account. &lt;p&gt; This value MUST be set.&lt;br/&gt; This value MUST be greater than the configured MIN_AUTORENEW_PERIOD.&lt;br/&gt; This value MUST be less than the configured MAX_AUTORENEW_PERIOD. |
| autoRenewAccount | [AccountID](#proto-AccountID) |  | The ID of the account to be charged renewal fees at the topic&#39;s expirationTime to extend the lifetime of the topic. &lt;p&gt; The topic lifetime SHALL be extended by the smallest of the following: &lt;ul&gt; &lt;li&gt;The current `autoRenewPeriod` duration.&lt;/li&gt; &lt;li&gt;The maximum duration that this account has funds to purchase.&lt;/li&gt; &lt;li&gt;The configured MAX_AUTORENEW_PERIOD at the time of automatic renewal.&lt;/li&gt; &lt;/ul&gt; If this value is set, the referenced account MUST sign this transaction.&lt;br/&gt; If this value is set, the `adminKey` field MUST also be set (though that key MAY not have any correlation to this account). |
| fee_schedule_key | [Key](#proto-Key) |  | Access control for update or delete of custom fees. &lt;p&gt; If set, subsequent `consensus_update_topic` transactions signed with this key MAY update or delete the custom fees for this topic.&lt;br/&gt; If not set, the custom fees for this topic SHALL BE immutable.&lt;br/&gt; If not set when the topic is created, this field CANNOT be set via update.&lt;br/&gt; If set when the topic is created, this field MAY be changed via update. |
| fee_exempt_key_list | [Key](#proto-Key) | repeated | A set of keys.&lt;br/&gt; Keys in this list are permitted to submit messages to this topic without paying custom fees associated with this topic. &lt;p&gt; If a submit transaction is signed by _any_ key included in this set, custom fees SHALL NOT be charged for that transaction.&lt;br/&gt; This field MUST NOT contain more than 10 keys.&lt;br/&gt; fee_exempt_key_list SHALL NOT contain any duplicate keys.&lt;br/&gt; fee_exempt_key_list MAY contain keys for accounts that are inactive, deleted, or non-existent.&lt;br/&gt; If fee_exempt_key_list is unset in this transaction, there SHALL NOT be any fee-exempt keys. In particular, the following keys SHALL NOT be implicitly or automatically added to this list: `adminKey`, `submitKey`, `fee_schedule_key`. |
| custom_fees | [FixedCustomFee](#proto-FixedCustomFee) | repeated | A set of custom fee definitions.&lt;br/&gt; These are fees to be assessed for each submit to this topic. &lt;p&gt; Each fee defined in this set SHALL be evaluated for each message submitted to this topic, and the resultant total assessed fees SHALL be charged.&lt;br/&gt; Custom fees defined here SHALL be assessed in addition to the base network and node fees.&lt;br/&gt; custom_fees list SHALL NOT contain more than `MAX_CUSTOM_FEE_ENTRIES_FOR_TOPICS` entries. |





 

 

 

 



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

