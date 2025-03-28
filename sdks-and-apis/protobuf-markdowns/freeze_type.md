# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/freeze_type.proto](#services_freeze_type-proto)
    - [FreezeType](#proto-FreezeType)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_freeze_type-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/freeze_type.proto
# Freeze Type
An enumeration to select the type of a network freeze.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).

 


<a name="proto-FreezeType"></a>

### FreezeType
An enumeration of possible network freeze types.

Each enumerated value SHALL be associated to a single network freeze
scenario. Each freeze scenario defines the specific parameters
REQUIRED for that freeze.

| Name | Number | Description |
| ---- | ------ | ----------- |
| UNKNOWN_FREEZE_TYPE | 0 | An invalid freeze type. &lt;p&gt; The first value in a protobuf enum is a default value. This default is RECOMMENDED to be an invalid value to aid in detecting unset fields. |
| FREEZE_ONLY | 1 | Freeze the network, and take no further action. &lt;p&gt; The `start_time` field is REQUIRED, MUST be strictly later than the consensus time when this transaction is handled, and SHOULD be between `300` and `3600` seconds after the transaction identifier `transactionValidStart` field.&lt;br/&gt; The fields `update_file` and `file_hash` SHALL be ignored.&lt;br/&gt; A `FREEZE_ONLY` transaction SHALL NOT perform any network changes or upgrades.&lt;br/&gt; After this freeze is processed manual intervention is REQUIRED to restart the network. |
| PREPARE_UPGRADE | 2 | This freeze type does not freeze the network, but begins &#34;preparation&#34; to upgrade the network. &lt;p&gt; The fields `update_file` and `file_hash` are REQUIRED and MUST be valid.&lt;br/&gt; The `start_time` field SHALL be ignored.&lt;br/&gt; A `PREPARE_UPGRADE` transaction SHALL NOT freeze the network or interfere with general transaction processing.&lt;br/&gt; If this freeze type is initiated after a `TELEMETRY_UPGRADE`, the prepared telemetry upgrade SHALL be reset and all telemetry upgrade artifacts in the filesystem SHALL be deleted.&lt;br/&gt; At some point after this freeze type completes (dependent on the size of the upgrade file), the network SHALL be prepared to complete a software upgrade of all nodes. |
| FREEZE_UPGRADE | 3 | Freeze the network to perform a software upgrade. &lt;p&gt; The `start_time` field is REQUIRED, MUST be strictly later than the consensus time when this transaction is handled, and SHOULD be between `300` and `3600` seconds after the transaction identifier `transactionValidStart` field.&lt;br/&gt; A software upgrade file MUST be prepared prior to this transaction.&lt;br/&gt; After this transaction completes, the network SHALL initiate an upgrade and restart of all nodes at the start time specified. |
| FREEZE_ABORT | 4 | Abort a pending network freeze operation. &lt;p&gt; All fields SHALL be ignored for this freeze type.&lt;br/&gt; This freeze type MAY be submitted after a `FREEZE_ONLY`, `FREEZE_UPGRADE`, or `TELEMETRY_UPGRADE` is initiated.&lt;br/&gt; This freeze type MUST be submitted and reach consensus before the `start_time` designated for the current pending freeze to be effective.&lt;br/&gt; After this freeze type is processed, the upgrade file hash and pending freeze start time stored in the network SHALL be reset to default (empty) values. |
| TELEMETRY_UPGRADE | 5 | Prepare an upgrade of auxiliary services and containers providing telemetry/metrics. &lt;p&gt; The `start_time` field is REQUIRED, MUST be strictly later than the consensus time when this transaction is handled, and SHOULD be between `300` and `3600` seconds after the transaction identifier `transactionValidStart` field.&lt;br/&gt; The `update_file` field is REQUIRED and MUST be valid.&lt;br/&gt; A `TELEMETRY_UPGRADE` transaction SHALL NOT freeze the network or interfere with general transaction processing.&lt;br/&gt; This freeze type MUST NOT be initiated between a `PREPARE_UPGRADE` and `FREEZE_UPGRADE`. If this freeze type is initiated after a `PREPARE_UPGRADE`, the prepared upgrade SHALL be reset and all software upgrade artifacts in the filesystem SHALL be deleted.&lt;br/&gt; At some point after this freeze type completes (dependent on the size of the upgrade file), the network SHALL automatically upgrade the telemetry/metrics services and containers as directed in the specified telemetry upgrade file. &lt;blockquote&gt; The condition that `start_time` is REQUIRED is an historical anomaly and SHOULD change in a future release.&lt;/blockquote&gt; |


 

 

 



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

