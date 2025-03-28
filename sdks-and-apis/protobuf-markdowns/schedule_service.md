# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/output/schedule_service.proto](#block_stream_output_schedule_service-proto)
    - [CreateScheduleOutput](#com-hedera-hapi-block-stream-output-CreateScheduleOutput)
    - [DeleteScheduleOutput](#com-hedera-hapi-block-stream-output-DeleteScheduleOutput)
    - [SignScheduleOutput](#com-hedera-hapi-block-stream-output-SignScheduleOutput)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_output_schedule_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/output/schedule_service.proto
# Schedule Service
Block stream messages that report the results of transactions handled by
the `Schedule` service.

The Schedule service handles delayed execution in two major forms.
- Transactions that only execute after being signed, via scheduleSign
  transactions, with additional keys beyond the keys that signe the
  scheduleCreate transaction.
- Transactions that only execute after a specified consensus time.

Some outputs contain a scheduled transaction identifier.&lt;br/&gt;
That value SHALL be set if, and only if, the transaction resulted in
_execution_ of the scheduled child transaction.&lt;br/&gt;
Most outputs from this service SHALL NOT set the scheduled transaction
identifier, because the transaction has not executed yet. Only when the
schedule has gathered all of the signatures required to execute the
scheduled child transaction is the network able to execute that child
transaction, and assign a final transaction identifier.

When a scheduled child transaction is executed, the identifier of the
schedule that executed that transaction SHALL be included as a
`schedule_ref` in the _result_ for the _child_ transaction.&lt;br/&gt;
The output of the schedule transaction (create or sign) SHALL NOT contain
the `schedule_ref`.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-output-CreateScheduleOutput"></a>

### CreateScheduleOutput
Block Stream data for a `createSchedule` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| schedule_id | [proto.ScheduleID](#proto-ScheduleID) |  | A schedule identifier. &lt;p&gt; If the status of the transaction is `SUCCESS`, this value SHALL be set to the identifier of the schedule that was created. If the transaction status is `IDENTICAL_SCHEDULE_ALREADY_CREATED`, this value SHALL be set to the identifier of the existing schedule that is identical to the one that was attempted to be created. For any other status, this value SHALL NOT be set. |
| scheduled_transaction_id | [proto.TransactionID](#proto-TransactionID) |  | A scheduled transaction identifier. &lt;p&gt; This value SHALL be the transaction identifier for the _scheduled_ child transaction executed as a result of gathering sufficient signatures to complete the schedule.&lt;br/&gt; This value SHALL NOT be set unless the scheduled transaction was executed as a child of this schedule create transaction.&lt;br/&gt; This value SHALL NOT be set unless this schedule create transaction was signed by sufficient keys to meet the signature requirements for the scheduled child transaction immediately. |






<a name="com-hedera-hapi-block-stream-output-DeleteScheduleOutput"></a>

### DeleteScheduleOutput
Block Stream data for a `deleteSchedule` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-SignScheduleOutput"></a>

### SignScheduleOutput
Block Stream data for a `signSchedule` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| scheduled_transaction_id | [proto.TransactionID](#proto-TransactionID) |  | A scheduled transaction identifier. &lt;p&gt; This value SHALL be the transaction identifier for the _scheduled_ child transaction executed as a result of gathering sufficient signatures to complete the schedule.&lt;br/&gt; This value SHALL NOT be set unless the scheduled transaction was executed as a child of this schedule sign transaction.&lt;br/&gt; This value SHALL NOT be set unless this schedule sign transaction was signed by sufficient additional keys to meet the signature requirements for the scheduled child transaction. |





 

 

 

 



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

