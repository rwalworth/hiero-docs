# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/schedule_create.proto](#services_schedule_create-proto)
    - [ScheduleCreateTransactionBody](#proto-ScheduleCreateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_schedule_create-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/schedule_create.proto
# Schedule Create
Message to create a schedule, which is an instruction to execute some other
transaction (the scheduled transaction) at a future time, either when
enough signatures are gathered (short term) or when the schedule expires
(long term). In all cases the scheduled transaction is not executed if
signature requirements are not met before the schedule expires.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ScheduleCreateTransactionBody"></a>

### ScheduleCreateTransactionBody
Create a new Schedule.

#### Requirements
This transaction SHALL create a new _schedule_ entity in network state.&lt;br/&gt;
The schedule created SHALL contain the `scheduledTransactionBody` to be
executed.&lt;br/&gt;
If successful the receipt SHALL contain a `scheduleID` with the full
identifier of the schedule created.&lt;br/&gt;
When a schedule _executes_ successfully, the receipt SHALL include a
`scheduledTransactionID` with the `TransactionID` of the transaction that
executed.&lt;br/&gt;
When a scheduled transaction is executed the network SHALL charge the
regular _service_ fee for the transaction to the `payerAccountID` for
that schedule, but SHALL NOT charge node or network fees.&lt;br/&gt;
If the `payerAccountID` field is not set, the effective `payerAccountID`
SHALL be the `payer` for this create transaction.&lt;br/&gt;
If an `adminKey` is not specified, or is an empty `KeyList`, the schedule
created SHALL be immutable.&lt;br/&gt;
An immutable schedule MAY be signed, and MAY execute, but SHALL NOT be
deleted.&lt;br/&gt;
If two schedules have the same values for all fields except `payerAccountID`
then those two schedules SHALL be deemed &#34;identical&#34;.&lt;br/&gt;
If a `scheduleCreate` requests a new schedule that is identical to an
existing schedule, the transaction SHALL fail and SHALL return a status
code of `IDENTICAL_SCHEDULE_ALREADY_CREATED` in the receipt.&lt;br/&gt;
The receipt for a duplicate schedule SHALL include the `ScheduleID` of the
existing schedule and the `TransactionID` of the earlier `scheduleCreate`
so that the earlier schedule may be queried and/or referred to in a
subsequent `scheduleSign`.

#### Signature Requirements
A `scheduleSign` transaction SHALL be used to add additional signatures
to an existing schedule.&lt;br/&gt;
Each signature SHALL &#34;activate&#34; the corresponding cryptographic(&#34;primitive&#34;)
key for that schedule.&lt;br/&gt;
Signature requirements SHALL be met when the set of active keys includes
all keys required by the scheduled transaction.&lt;br/&gt;
A scheduled transaction for a &#34;long term&#34; schedule SHALL NOT execute if
the signature requirements for that transaction are not met when the
network consensus time reaches the schedule `expiration_time`.&lt;br/&gt;
A &#34;short term&#34; schedule SHALL execute immediately once signature
requirements are met. This MAY be immediately when created.

#### Long Term Schedules
A &#34;short term&#34; schedule SHALL have the flag `wait_for_expiry` _unset_.&lt;br/&gt;
A &#34;long term&#34; schedule SHALL have the flag  `wait_for_expiry` _set_.&lt;br/&gt;
A &#34;long term&#34; schedule SHALL NOT be accepted if the network configuration
`scheduling.longTermEnabled` is not enabled.&lt;br/&gt;
A &#34;long term&#34; schedule SHALL execute when the current consensus time
matches or exceeds the `expiration_time` for that schedule, if the
signature requirements for the scheduled transaction
are met at that instant.&lt;br/&gt;
A &#34;long term&#34; schedule SHALL NOT execute before the current consensus time
matches or exceeds the `expiration_time` for that schedule.&lt;br/&gt;
A &#34;long term&#34; schedule SHALL expire, and be removed from state, after the
network consensus time exceeds the schedule `expiration_time`.&lt;br/&gt;
A short term schedule SHALL expire, and be removed from state,
after the network consensus time exceeds the current network
configuration for `ledger.scheduleTxExpiryTimeSecs`.

&gt; Note
&gt;&gt; Long term schedules are not (as of release 0.56.0) enabled. Any schedule
&gt;&gt; created currently MUST NOT set the `wait_for_expiry` flag.&lt;br/&gt;
&gt;&gt; When long term schedules are not enabled, schedules SHALL NOT be
&gt;&gt; executed at expiration, and MUST meet signature requirements strictly
&gt;&gt; before expiration to be executed.

### Block Stream Effects
If the scheduled transaction is executed immediately, the transaction
record SHALL include a `scheduleRef` with the schedule identifier of the
schedule created.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| scheduledTransactionBody | [SchedulableTransactionBody](#proto-SchedulableTransactionBody) |  | A scheduled transaction. &lt;p&gt; This value is REQUIRED.&lt;br/&gt; This transaction body MUST be one of the types enabled in the network configuration value `scheduling.whitelist`. |
| memo | [string](#string) |  | A short description of the schedule. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| adminKey | [Key](#proto-Key) |  | A `Key` required to delete this schedule. &lt;p&gt; If this is not set, or is an empty `KeyList`, this schedule SHALL be immutable and SHALL NOT be deleted. |
| payerAccountID | [AccountID](#proto-AccountID) |  | An account identifier of a `payer` for the scheduled transaction. &lt;p&gt; This value MAY be unset. If unset, the `payer` for this `scheduleCreate` transaction SHALL be the `payer` for the scheduled transaction.&lt;br/&gt; If this is set, the identified account SHALL be charged the fees required for the scheduled transaction when it is executed.&lt;br/&gt; If the actual `payer` for the _scheduled_ transaction lacks sufficient HBAR balance to pay service fees for the scheduled transaction _when it executes_, the scheduled transaction SHALL fail with `INSUFFICIENT_PAYER_BALANCE`.&lt;br/&gt; |
| expiration_time | [Timestamp](#proto-Timestamp) |  | An expiration time. &lt;p&gt; If not set, the expiration SHALL default to the current consensus time advanced by either the network configuration value `scheduling.maxExpirationFutureSeconds`, if `wait_for_expiry` is set and &#34;long term&#34; schedules are enabled, or the network configuration value `ledger.scheduleTxExpiryTimeSecs` otherwise. |
| wait_for_expiry | [bool](#bool) |  | A flag to delay execution until expiration. &lt;p&gt; If this flag is set the scheduled transaction SHALL NOT be evaluated for execution before the network consensus time matches or exceeds the `expiration_time`.&lt;br/&gt; If this flag is not set, the scheduled transaction SHALL be executed immediately when all required signatures are received, whether in this `scheduleCreate` transaction or a later `scheduleSign` transaction.&lt;br/&gt; This value SHALL NOT be used and MUST NOT be set when the network configuration value `scheduling.longTermEnabled` is not enabled. |





 

 

 

 



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

