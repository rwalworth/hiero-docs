# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [platform/event/event_transaction.proto](#platform_event_event_transaction-proto)
    - [EventTransaction](#com-hedera-hapi-platform-event-EventTransaction)
  
    - [TransactionGroupRole](#com-hedera-hapi-platform-event-TransactionGroupRole)
  
- [Scalar Value Types](#scalar-value-types)



<a name="platform_event_event_transaction-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## platform/event/event_transaction.proto
# Event Transaction
An Event Transaction gossiped between nodes as part of events.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-platform-event-EventTransaction"></a>

### EventTransaction
An Event Transaction gossiped between nodes as part of events.

Each node MUST extract this transaction and process according to the type
of transaction encoded.&lt;br/&gt;
Both the platform and the application built on that platform MAY define event
transactions.&lt;br/&gt;
The encoded data MUST be a serialized protobuf message.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| application_transaction | [bytes](#bytes) |  | An application transaction. &lt;p&gt; The contents of this transaction SHALL be defined by the application subsystem that created the event.&lt;br/&gt; The contents MUST be a serialized protobuf message. |
| state_signature_transaction | [StateSignatureTransaction](#com-hedera-hapi-platform-event-StateSignatureTransaction) |  | A state signature. &lt;p&gt; This transaction SHALL be a valid state signature for a state snapshot. |
| transaction_group_role | [TransactionGroupRole](#com-hedera-hapi-platform-event-TransactionGroupRole) |  | The role of this transaction in a group of transactions. |





 


<a name="com-hedera-hapi-platform-event-TransactionGroupRole"></a>

### TransactionGroupRole
Defines the position of an EventTransaction relative to a logical &#34;group&#34;
of EventTransaction that correspond to a single set of changes to the state
Merkle tree.

| Name | Number | Description |
| ---- | ------ | ----------- |
| STANDALONE | 0 | The EventTransaction is the only transaction in its group. |
| FIRST_CHILD | 1 | The EventTransaction is a child and first in its group. |
| MIDDLE_CHILD | 2 | The EventTransaction is a child in the middle of its group. |
| LAST_CHILD | 3 | The EventTransaction is a child and last in its group. |
| STARTING_PARENT | 4 | The EventTransaction is a parent and first in its group. |
| PARENT | 5 | The EventTransaction is a parent in the middle of its group. |
| ENDING_PARENT | 6 | The EventTransaction is a parent and last in its group. |


 

 

 



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

