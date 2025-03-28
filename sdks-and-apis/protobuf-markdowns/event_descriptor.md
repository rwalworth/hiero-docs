# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [platform/event/event_descriptor.proto](#platform_event_event_descriptor-proto)
    - [EventDescriptor](#com-hedera-hapi-platform-event-EventDescriptor)
  
- [Scalar Value Types](#scalar-value-types)



<a name="platform_event_event_descriptor-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## platform/event/event_descriptor.proto
# Event Descriptor
Unique identifier for an event.

Contains the hash of the event, the creator identifier, the birth round, and the generation.

An event&#39;s descriptor is constructed individually by each node that receives a `GossipEvent`,
to uniquely identify that event. An event&#39;s descriptor isn&#39;t part of the `GossipEvent` itself,
since the descriptor contains the fields `hash` and `generation`, which can be computed locally.
Nodes receiving a `GossipEvent` have the required information to construct the event descriptor
immediately upon receiving the event, without needing to wait for the event to reach consensus.

Aside from being a unique identifier for events that have been received through gossip,
the event descriptor contains the necessary information to describe an event&#39;s parents,
in the `parents` field of `GossipEvent`.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119).


<a name="com-hedera-hapi-platform-event-EventDescriptor"></a>

### EventDescriptor
Unique identifier for an event.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hash | [bytes](#bytes) |  | The hash of the event.&lt;br/&gt; The hash SHALL be a SHA-384 hash.&lt;br/&gt; The hash SHALL have the following inputs, in the specified order:&lt;br/&gt; 1. The bytes of the `EventCore` protobuf&lt;br/&gt; 2. The SHA-384 hash of each individual `EventTransaction`, in the order the transactions appear in the `event_transactions` field of the `GossipEvent` protobuf |
| creator_node_id | [int64](#int64) |  | The creator node identifier.&lt;br/&gt; This SHALL be the unique identifier for the node that created the event.&lt;br/&gt; This SHALL match the ID of the node as it appears in the address book. |
| birth_round | [int64](#int64) |  | The birth round of the event.&lt;br/&gt; The birth round SHALL be the pending consensus round at the time the event is created.&lt;br/&gt; The pending consensus round SHALL be **one greater** than the latest round to reach consensus. |
| generation | [int64](#int64) |  | The generation of the event.&lt;br/&gt; This value SHALL be **one greater** than the _maximum_ generation of all parents.&lt;br/&gt;

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

