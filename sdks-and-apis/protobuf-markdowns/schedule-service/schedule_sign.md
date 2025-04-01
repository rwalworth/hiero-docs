# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/schedule_sign.proto](#services_schedule_sign-proto)
    - [ScheduleSignTransactionBody](#proto-ScheduleSignTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_schedule_sign-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/schedule_sign.proto
# Schedule Sign
Transaction body for a `scheduleSign` transaction to add signatures
to an existing scheduled transaction.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ScheduleSignTransactionBody"></a>

### ScheduleSignTransactionBody
Add signatures to an existing scheduled transaction.

When a schedule _executes_ successfully, the receipt SHALL include a
`scheduledTransactionID` with the `TransactionID` of the transaction that
executed.&lt;br/&gt;
When a scheduled transaction is executed the network SHALL charge the
regular _service_ fee for the transaction to the `payerAccountID` for
that schedule, but SHALL NOT charge node or network fees.&lt;br/&gt;
If the `payerAccountID` field is not set, the effective `payerAccountID`
SHALL be the `payer` for this create transaction.&lt;br/&gt;
Each signature on this transaction SHALL &#34;activate&#34; the corresponding
cryptographic(&#34;primitive&#34;) key for the schedule identified.&lt;br/&gt;
Signature requirements SHALL be met when the set of active keys includes
all keys required by the scheduled transaction.&lt;br/&gt;
A scheduled transaction for a &#34;long term&#34; schedule SHALL NOT execute if
the signature requirements for that transaction are not met when the
network consensus time reaches the schedule `expiration_time`.&lt;br/&gt;
A &#34;short term&#34; schedule SHALL execute immediately once signature
requirements are met. This MAY be immediately when created.&lt;br/&gt;

### Block Stream Effects
If the scheduled transaction is executed immediately following this
`scheduleSign` transaction, the transaction record SHALL include a
`scheduleRef` with the schedule identifier `scheduleID`.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| scheduleID | [ScheduleID](#proto-ScheduleID) |  | A schedule identifier. &lt;p&gt; This MUST identify the schedule to which signatures SHALL be added. |





 

 

 

 



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

