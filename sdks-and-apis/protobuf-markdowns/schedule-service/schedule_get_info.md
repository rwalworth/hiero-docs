# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/schedule_get_info.proto](#services_schedule_get_info-proto)
    - [ScheduleGetInfoQuery](#proto-ScheduleGetInfoQuery)
    - [ScheduleGetInfoResponse](#proto-ScheduleGetInfoResponse)
    - [ScheduleInfo](#proto-ScheduleInfo)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_schedule_get_info-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/schedule_get_info.proto
# Schedule Get Information
Query body and response to retrieve information about a scheduled
transaction.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ScheduleGetInfoQuery"></a>

### ScheduleGetInfoQuery
Request for information about a scheduled transaction.

If the requested schedule does not exist, the network SHALL respond
with `INVALID_SCHEDULE_ID`.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| scheduleID | [ScheduleID](#proto-ScheduleID) |  | A schedule identifier. &lt;p&gt; This SHALL identify the schedule to retrieve.&lt;br/&gt; This field is REQUIRED. |






<a name="proto-ScheduleGetInfoResponse"></a>

### ScheduleGetInfoResponse
A response message for a `getScheduleInfo` query.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| scheduleInfo | [ScheduleInfo](#proto-ScheduleInfo) |  | Detail information for a schedule. &lt;p&gt; This field SHALL contain all available schedule detail. |






<a name="proto-ScheduleInfo"></a>

### ScheduleInfo
Information summarizing schedule state


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| scheduleID | [ScheduleID](#proto-ScheduleID) |  | A schedule identifier. &lt;p&gt; This SHALL identify the schedule retrieved. |
| deletion_time | [Timestamp](#proto-Timestamp) |  | A deletion timestamp. &lt;p&gt; If the schedule was deleted, this SHALL be set to the consensus timestamp of the `deleteSchedule` transaction.&lt;br/&gt; If the schedule is _not_ deleted, this field SHALL NOT be set. |
| execution_time | [Timestamp](#proto-Timestamp) |  | An execution timestamp. &lt;p&gt; If the schedule was completed, and the _scheduled_ transaction executed, this SHALL be set to the consensus timestamp of the transaction that initiated that execution.&lt;br/&gt; If the schedule is _not_ complete, this field SHALL NOT be set. |
| expirationTime | [Timestamp](#proto-Timestamp) |  | An expiration timestamp.&lt;br/&gt; This represents the time at which the schedule will expire. For a long-term schedule (if enabled) this is when the schedule will be executed, assuming it meets signature requirements at that time. For a short-term schedule, this is the deadline to complete the signature requirements for the scheduled transaction to execute. Regardless of schedule type, the schedule will be removed from state when it expires. &lt;p&gt; A schedule SHALL be removed from state when it expires.&lt;br/&gt; A short-term schedule MUST meet signature requirements strictly before expiration or it SHALL NOT be executed.&lt;br/&gt; A long-term schedule SHALL be executed if, and only if, all signature requirements for the scheduled transaction are met at expiration.&lt;br/&gt; A long-term schedule SHALL NOT be executed if any signature requirement for the scheduled transaction are not met at expiration.&lt;br/&gt; |
| scheduledTransactionBody | [SchedulableTransactionBody](#proto-SchedulableTransactionBody) |  | A scheduled transaction. &lt;p&gt; This SHALL be a transaction type enabled in the network property `scheduling.whitelist`, and SHALL NOT be any other transaction type.&lt;br/&gt; This transaction SHALL be executed if the schedule meets all signature and execution time requirements for this transaction.&lt;br/&gt; The signature requirements for this transaction SHALL be evaluated at schedule creation, SHALL be reevaluated with each `signSchedule` transaction, and, for long-term schedules, SHALL be reevaluated when the schedule expires.&lt;br/&gt; |
| memo | [string](#string) |  | A short description for this schedule. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| adminKey | [Key](#proto-Key) |  | The key used to delete the schedule from state |
| signers | [KeyList](#proto-KeyList) |  | A list of &#34;valid&#34; signatures for this schedule.&lt;br/&gt; This list contains only &#34;primitive&#34; (i.e. cryptographic or contract) signatures. The full signature requirements for the scheduled transaction are evaluated as if this list of keys had signed the scheduled transaction directly. &lt;p&gt; This list SHALL contain every &#34;primitive&#34; key that has signed the original `createSchedule`, or any subsequent `signSchedule` transaction.&lt;br/&gt; This list MAY elide any signature not likely to be required by the scheduled transaction. Such requirement SHOULD be evaluated when the signature is presented (i.e. during evaluation of a `createSchedule` or `signSchedule` transaction). |
| creatorAccountID | [AccountID](#proto-AccountID) |  | An account identifier. &lt;p&gt; This SHALL identify the account that created this schedule. |
| payerAccountID | [AccountID](#proto-AccountID) |  | An account identifier. &lt;p&gt; The identified account SHALL pay the full transaction fee for the scheduled transaction _when it executes_. |
| scheduledTransactionID | [TransactionID](#proto-TransactionID) |  | A transaction identifier. &lt;p&gt; This SHALL be recorded as the transaction identifier for the _scheduled_ transaction, if (and when) it is executed. |
| ledger_id | [bytes](#bytes) |  | The ledger ID of the network that generated this response. &lt;p&gt; This value SHALL identify the distributed ledger that responded to this query. |
| wait_for_expiry | [bool](#bool) |  | A flag indicating this schedule will execute when it expires. &lt;p&gt; If this field is set &lt;ul&gt; &lt;li&gt;This schedule SHALL be considered a &#34;long-term&#34; schedule.&lt;/li&gt; &lt;li&gt;This schedule SHALL be evaluated when the network consensus time reaches the `expirationTime`, and if the signature requirements for the scheduled transaction are met at that time, the scheduled transaction SHALL be executed.&lt;/li&gt; &lt;li&gt;This schedule SHALL NOT be executed before the network consensus time reaches the `expirationTime`.&lt;/li&gt; &lt;/ul&gt; If this field is not set &lt;ul&gt; &lt;li&gt;This schedule SHALL be considered a &#34;short-term&#34; schedule.&lt;/li&gt; &lt;li&gt;This schedule SHALL be evaluated when created, and reevaluated with each `signSchedule` transaction, and if the signature requirements for the scheduled transaction are met at that time, the scheduled transaction SHALL be executed immediately.&lt;/li&gt; &lt;li&gt;This schedule SHALL be executed as soon as the signature requirements are met, and MUST be executed before the network consensus time reaches the `expirationTime`, if at all.&lt;/li&gt; &lt;/ul&gt; |





 

 

 

 



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

