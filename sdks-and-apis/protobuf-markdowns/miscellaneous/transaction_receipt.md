# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/transaction_receipt.proto](#services_transaction_receipt-proto)
    - [TransactionReceipt](#proto-TransactionReceipt)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_transaction_receipt-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/transaction_receipt.proto
# Transaction Receipt
The receipt returned when the results of a transaction are requested via
`TransactionGetReceiptQuery`.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TransactionReceipt"></a>

### TransactionReceipt
The summary of a transaction&#39;s result so far.&lt;br/&gt;
If the transaction has not reached consensus, this result will
be necessarily incomplete.

Most items in this object are only set for specific transactions.
Those values SHALL be unset for all other transactions.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| status | [ResponseCodeEnum](#proto-ResponseCodeEnum) |  | The consensus status of the transaction. &lt;p&gt; This SHALL be `UNKNOWN` if consensus has not been reached.&lt;br/&gt; This SHALL be `UNKNOWN` if the associated transaction did not have a valid payer signature. |
| accountID | [AccountID](#proto-AccountID) |  | In the receipt of a `CryptoCreate`, the id of the newly created account. |
| fileID | [FileID](#proto-FileID) |  | In the receipt of a `FileCreate`, the id of the newly created file. |
| contractID | [ContractID](#proto-ContractID) |  | In the receipt of a `ContractCreate`, the id of the newly created contract. |
| exchangeRate | [ExchangeRateSet](#proto-ExchangeRateSet) |  | The exchange rates in effect when the transaction reached consensus. |
| topicID | [TopicID](#proto-TopicID) |  | In the receipt of a `ConsensusCreateTopic`, the id of the newly created topic. |
| topicSequenceNumber | [uint64](#uint64) |  | In the receipt of a `ConsensusSubmitMessage`, the new sequence number for the topic that received the message. |
| topicRunningHash | [bytes](#bytes) |  | In the receipt of a `ConsensusSubmitMessage`, the new running hash of the topic that received the message.&lt;br/&gt; &lt;p&gt; The inputs to the topic running hash have changed over time.&lt;br/&gt; This 48-byte field is the output of a SHA-384 digest with input data determined by the value of the `topicRunningHashVersion` field.&lt;br/&gt; All new transactions SHALL use `topicRunningHashVersion` `3`.&lt;br/&gt; The bytes of each uint64 or uint32 encoded for the hash input MUST be in Big-Endian format. &lt;p&gt; &lt;hr style=&#34;margin: 0.2em 5em 0.2em 5em; height: 0.5em; border-style: solid none solid none; border-width: 2px;&#34;/&gt; &lt;p&gt; The most recent version is denoted by `topicRunningHashVersion = 3`. &lt;p&gt; This version SHALL include, in order &lt;ol&gt; &lt;li&gt;The previous running hash of the topic (48 bytes)&lt;/li&gt; &lt;li&gt;The `topic_running_hash_version` field (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The payer account&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The number of seconds since the epoch when the `ConsensusSubmitMessage` reached consensus (8 bytes)&lt;/li&gt; &lt;li&gt;The number of nanoseconds within the second when the `ConsensusSubmitMessage` reached consensus (4 bytes)&lt;/li&gt; &lt;li&gt;The `topic_sequence_number` field (8 bytes)&lt;/li&gt; &lt;li&gt;The output of a SHA-384 digest of the message bytes from the `ConsensusSubmitMessage` (48 bytes)&lt;/li&gt; &lt;/ol&gt; &lt;hr style=&#34;margin: 0.2em 5em 0.2em 5em; height: 0.5em; border-style: solid none solid none; border-width: 2px;&#34;/&gt; &lt;p&gt; The next older version is denoted by `topicRunningHashVersion = 2`. &lt;p&gt; This version SHALL include, in order &lt;ol&gt; &lt;li&gt;The previous running hash of the topic (48 bytes)&lt;/li&gt; &lt;li&gt;The `topic_running_hash_version` field (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The number of seconds since the epoch when the `ConsensusSubmitMessage` reached consensus (8 bytes)&lt;/li&gt; &lt;li&gt;The number of nanoseconds within the second when the `ConsensusSubmitMessage` reached consensus (4 bytes)&lt;/li&gt; &lt;li&gt;The `topic_sequence_number` field (8 bytes)&lt;/li&gt; &lt;li&gt;The output of a SHA-384 digest of the message bytes from the `ConsensusSubmitMessage` (48 bytes)&lt;/li&gt; &lt;/ol&gt; &lt;hr style=&#34;margin: 0.2em 5em 0.2em 5em; height: 0.5em; border-style: solid none solid none; border-width: 2px;&#34;/&gt; &lt;p&gt; The original version, used at genesis, is denoted by `topicRunningHashVersion = 1` or `topicRunningHashVersion = 0`. &lt;p&gt; This version SHALL include, in order &lt;ol&gt; &lt;li&gt;The previous running hash of the topic (48 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s shard (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s realm (8 bytes)&lt;/li&gt; &lt;li&gt;The topic&#39;s number (8 bytes)&lt;/li&gt; &lt;li&gt;The number of seconds since the epoch when the `ConsensusSubmitMessage` reached consensus (8 bytes)&lt;/li&gt; &lt;li&gt;The number of nanoseconds within the second when the `ConsensusSubmitMessage` reached consensus (4 bytes)&lt;/li&gt; &lt;li&gt;The `topic_sequence_number` field (8 bytes)&lt;/li&gt; &lt;li&gt;The message bytes from the `ConsensusSubmitMessage` (variable)&lt;/li&gt; &lt;/ol&gt; |
| topicRunningHashVersion | [uint64](#uint64) |  | In the receipt of a `ConsensusSubmitMessage`, the version of the SHA-384 digest inputs used to update the running hash. |
| tokenID | [TokenID](#proto-TokenID) |  | In the receipt of a `CreateToken`, the id of the newly created token |
| newTotalSupply | [uint64](#uint64) |  | In the receipt of `TokenMint`, `TokenWipe`, or `TokenBurn`.&lt;br/&gt; For non-unique tokens, the current total supply of that token.&lt;br/&gt; For unique tokens,the total number of NFTs issued for that token. |
| scheduleID | [ScheduleID](#proto-ScheduleID) |  | In the receipt of a `ScheduleCreate`, the id of the newly created Scheduled Entity |
| scheduledTransactionID | [TransactionID](#proto-TransactionID) |  | In the receipt of a `ScheduleCreate` or `ScheduleSign` that enables the scheduled transaction to execute immediately, the `TransactionID` that should be used to query for the receipt or record of the scheduled transaction that was executed. |
| serialNumbers | [int64](#int64) | repeated | In the receipt of a `TokenMint` for non-fungible/unique tokens, the serial numbers of the newly created tokens. |
| node_id | [uint64](#uint64) |  | An affected node identifier.&lt;br/&gt; In the receipt of a NodeCreate, the id of the newly created node. &lt;p&gt; This value SHALL be set following a `createNode` transaction.&lt;br/&gt; This value SHALL NOT be set following any other transaction. |





 

 

 

 



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

