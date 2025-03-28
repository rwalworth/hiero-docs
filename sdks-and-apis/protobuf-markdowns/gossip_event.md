# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [platform/event/gossip_event.proto](#platform_event_gossip_event-proto)
    - [GossipEvent](#com-hedera-hapi-platform-event-GossipEvent)
  
- [Scalar Value Types](#scalar-value-types)



<a name="platform_event_gossip_event-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## platform/event/gossip_event.proto
# Gossip Event
An event that is sent and received via gossip

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119).


<a name="com-hedera-hapi-platform-event-GossipEvent"></a>

### GossipEvent
An event that is sent and received via gossip


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| event_core | [EventCore](#com-hedera-hapi-platform-event-EventCore) |  | The core event data |
| signature | [bytes](#bytes) |  | A node signature on the event hash.&lt;br/&gt; The signature SHALL be created with the SHA384withRSA algorithm.&lt;br/&gt; The signature MUST verify using the public key belonging to the `event_creator`.&lt;br/&gt; The `event_creator` public key SHALL be read from the address book that corresponds to the event&#39;s birth round.&lt;br/&gt; The signed event hash SHALL be a SHA-384 hash.&lt;br/&gt; The signed event hash SHALL have the following inputs, in the specified order:&lt;br/&gt; 1. The bytes of the `event_core` field&lt;br/&gt; 2. The SHA-384 hash of each individual `EventTransaction`, in the order the transaction appear in the `event_transaction` field |
| transactions | [bytes](#bytes) | repeated | A list of serialized transactions. &lt;p&gt; This field MAY contain zero transactions.&lt;br/&gt; Each transaction in this list SHALL be presented exactly as it was supplied to the consensus algorithm.&lt;br/&gt; This field MUST contain one entry for each transaction included in this gossip event. |





 

 

 

 



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

