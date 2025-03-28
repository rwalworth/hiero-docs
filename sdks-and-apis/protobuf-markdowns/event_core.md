# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [platform/event/event_core.proto](#platform_event_event_core-proto)
    - [EventCore](#com-hedera-hapi-platform-event-EventCore)
  
- [Scalar Value Types](#scalar-value-types)



<a name="platform_event_event_core-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## platform/event/event_core.proto
# Core Event Data
A message that describes the metadata for an event.

The `EventCore` contains a list of the event&#39;s parents, as well as the software
version, an identifier for the node that created this event, the birth round, and
the creation timestamp for the event.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119).


<a name="com-hedera-hapi-platform-event-EventCore"></a>

### EventCore
Contains information about an event and its parents.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| creator_node_id | [int64](#int64) |  | The creator node identifier.&lt;br/&gt; This SHALL be the unique identifier for the node that created the event.&lt;br/&gt; This SHALL match the ID of the node as it appears in the address book. |
| birth_round | [int64](#int64) |  | The birth round of the event.&lt;br/&gt; The birth round SHALL be the pending consensus round at the time the event is created.&lt;br/&gt; The pending consensus round SHALL be **one greater** than the latest round to reach consensus. |
| time_created | [proto.Timestamp](#proto-Timestamp) |  | The wall clock time at which the event was created, according to the node creating the event.&lt;br/&gt; If the event has a self parent, this timestamp MUST be strictly greater than the `time_created` of the self parent. |
| parents | [EventDescriptor](#com-hedera-hapi-platform-event-EventDescriptor) | repeated | A list of EventDescriptors representing the parents of this event.&lt;br/&gt; The list of parents SHALL include zero or one self parents, and zero or more other parents.&lt;br/&gt; The first element of the list SHALL be the self parent, if one exists.&lt;br/&gt; The list of parents SHALL NOT include more than one parent from the same creator. |
| version | [proto.SemanticVersion](#proto-SemanticVersion) |  | The event specification version.&lt;br/&gt; The specification described by this version SHALL encompass the format of the `GossipEvent` message, and also the format of all contained messages.&lt;br/&gt; This SHALL exactly match the specification version passed into the platform at construction.

This field is temporary until birth_round migration is complete. Field number 17 chosen to avoid polluting cheaper 1 byte field numbers 1-16 |





 

 

 

 



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

