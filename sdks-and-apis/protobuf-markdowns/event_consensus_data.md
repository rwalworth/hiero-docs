# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [platform/event/event_consensus_data.proto](#platform_event_event_consensus_data-proto)
    - [EventConsensusData](#com-hedera-hapi-platform-event-EventConsensusData)
  
- [Scalar Value Types](#scalar-value-types)



<a name="platform_event_event_consensus_data-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## platform/event/event_consensus_data.proto
# Event Consensus Data
A message that describes the consensus data for an event.

The `EventConsensusData` contains two fields that are determined once an
event reaches consensus, the `consensus_timestamp` and `consensus_order`.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119).


<a name="com-hedera-hapi-platform-event-EventConsensusData"></a>

### EventConsensusData
Event Consensus Data.&lt;br/&gt;
This message records the critical values produced by consensus for an event.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| consensus_timestamp | [proto.Timestamp](#proto-Timestamp) |  | A consensus timestamp.&lt;br/&gt; The network&#39;s consensus agreement on a timestamp for this event. &lt;p&gt; This timestamp MUST be strictly greater than the `consensus_timestamp` of the previous consensus event.&lt;br/&gt; This is a consensus value and MAY NOT match real-world &#34;wall clock&#34; time. |
| consensus_order | [uint64](#uint64) |  | A consensus order sequence number.&lt;br/&gt; A non-negative sequence number that identifies an event&#39;s consensus order since genesis. &lt;p&gt; This SHALL be the unique for each consensus event.&lt;br/&gt; This SHALL always increase, and SHALL NOT decrease.&lt;br/&gt; This SHALL increment by one for each consensus event. |





 

 

 

 



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

