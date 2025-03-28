# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/throttles/throttle_usage_snapshots.proto](#services_state_throttles_throttle_usage_snapshots-proto)
    - [ThrottleUsageSnapshot](#proto-ThrottleUsageSnapshot)
    - [ThrottleUsageSnapshots](#proto-ThrottleUsageSnapshots)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_throttles_throttle_usage_snapshots-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/throttles/throttle_usage_snapshots.proto
# Throttle Snapshots
Point-in-time information regarding throttle usage.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ThrottleUsageSnapshot"></a>

### ThrottleUsageSnapshot
A single snapshot of the used throttle capacity for a throttle and point in
time.

&gt; Question:
&gt;&gt; What throttle does this apply to? How is that determined?


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| used | [int64](#int64) |  | Used throttle capacity. |
| last_decision_time | [Timestamp](#proto-Timestamp) |  | The time at which the this snapshot of capacity was calculated.&lt;br/&gt; Stored as an offset from the `epoch`. &lt;p&gt; For this purpose, `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`. |






<a name="proto-ThrottleUsageSnapshots"></a>

### ThrottleUsageSnapshots
All point-in-time snapshots of throttle usage for TPS and &#34;gas&#34; throttle
values for a given point in time.

&gt; Question:
&gt;&gt; What point in time?  Should this store consensus timestamp here?


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tps_throttles | [ThrottleUsageSnapshot](#proto-ThrottleUsageSnapshot) | repeated | A list of snapshots for TPS throttles. &lt;p&gt; &lt;blockquote&gt;Question:&lt;blockquote&gt;What is the order?&lt;/blockquote&gt;&lt;/blockquote&gt; |
| gas_throttle | [ThrottleUsageSnapshot](#proto-ThrottleUsageSnapshot) |  | A single snapshot for the gas throttle. |





 

 

 

 



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

