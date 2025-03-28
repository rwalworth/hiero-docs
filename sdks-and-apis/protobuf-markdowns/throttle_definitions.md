# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/throttle_definitions.proto](#services_throttle_definitions-proto)
    - [ThrottleBucket](#proto-ThrottleBucket)
    - [ThrottleDefinitions](#proto-ThrottleDefinitions)
    - [ThrottleGroup](#proto-ThrottleGroup)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_throttle_definitions-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/throttle_definitions.proto
# Throttle Definitions
A set of messages that support maintaining throttling limits on network
transactions to ensure no one transaction type consumes the entirety of
network resources. Also used to charge congestion fees when network load
is exceptionally high, as an incentive to delay transactions that are
not time-sensitive.

For details behind this throttling design, please see the
`docs/throttle-design.md` document in the
[Hedera Services](https://github.com/hashgraph/hedera-services) repository.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ThrottleBucket"></a>

### ThrottleBucket
A &#34;bucket&#34; of performance allocated across one or more throttle groups.&lt;br/&gt;
This entry combines one or more throttle groups into a single unit to
calculate limitations and congestion. Each &#34;bucket&#34; &#34;fills&#34; as operations
are completed, then &#34;drains&#34; over a period of time defined for each bucket.
This fill-and-drain characteristic enables the network to process sudden
bursts of heavy traffic while still observing throttle limits over longer
timeframes.

The value of `burstPeriodMs` is combined with the `milliOpsPerSec`
values for the individual throttle groups to determine the total
bucket &#34;capacity&#34;. This combination MUST be less than the maximum
value of a signed long integer (`9223372036854775807`), when scaled to
a nanosecond measurement resolution.

&gt; Note
&gt;&gt; There is some question regarding the mechanism of calculating the
&gt;&gt; combination of `burstPeriodMs` and `milliOpsPerSec`. The calculation
&gt;&gt; Is implemented in difficult-to-find code, and very likely does not
&gt;&gt; match the approach described here.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | A name for this bucket.&lt;br/&gt; This is used for log entries. &lt;p&gt; This value SHOULD NOT exceed 20 characters. |
| burstPeriodMs | [uint64](#uint64) |  | A burst duration limit, in milliseconds.&lt;br/&gt; This value determines the total &#34;capacity&#34; of the bucket. The rate at which the bucket &#34;drains&#34; is set by the throttles, and this duration sets how long that rate must be sustained to empty a &#34;full&#34; bucket. That combination (calculated as the product of this value and the least common multiple of the `milliOpsPerSec` values for all throttle groups) determines the maximum amount of operations this bucket can &#34;hold&#34;. &lt;p&gt; The calculated capacity of this bucket MUST NOT exceed `9,223,372,036,854`. |
| throttleGroups | [ThrottleGroup](#proto-ThrottleGroup) | repeated | A list of throttle groups.&lt;br/&gt; These throttle groups combined define the effective throttle rate for the bucket. &lt;p&gt; This list MUST contain at least one entry. |






<a name="proto-ThrottleDefinitions"></a>

### ThrottleDefinitions
A list of throttle buckets.&lt;br/&gt;
This list, simultaneously enforced, defines a complete throttling policy.

 1. When an operation appears in more than one throttling bucket,
    that operation SHALL be throttled unless all of the buckets where
    the operation appears have &#34;capacity&#34; available.
 1. An operation assigned to no buckets is SHALL be throttled in every
    instance.  The _effective_ throttle for this case is `0`.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| throttleBuckets | [ThrottleBucket](#proto-ThrottleBucket) | repeated | A list of throttle buckets. &lt;p&gt; This list MUST be set, and SHOULD NOT be empty.&lt;br/&gt; An empty list SHALL have the effect of setting all operations to a single group with throttle limit of `0` operations per second for the entire network. |






<a name="proto-ThrottleGroup"></a>

### ThrottleGroup
A single throttle limit applied to one or more operations.

The list of operations MUST contain at least one entry.&lt;br/&gt;
The throttle limit SHALL be specified in thousandths of an operation
per second; one operation per second for the network would be `1000`.&lt;br/&gt;
The throttle limit MUST be greater than zero (`0`).


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| operations | [HederaFunctionality](#proto-HederaFunctionality) | repeated | A list of operations to be throttled. &lt;p&gt; This list MUST contain at least one item.&lt;br/&gt; This list SHOULD NOT contain any item included in any other active `ThrottleGroup`. |
| milliOpsPerSec | [uint64](#uint64) |  | A throttle limit for this group.&lt;br/&gt; This is a total number of operations, in thousandths, the network may perform each second for this group. Every node executes every transaction, so this limit effectively applies individually to each node as well.&lt;br/&gt; &lt;p&gt; This value MUST be greater than zero (`0`).&lt;br/&gt; This value SHOULD be less than `9,223,372`.&lt;br/&gt; |





 

 

 

 



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

