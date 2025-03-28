# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/freeze.proto](#services_freeze-proto)
    - [FreezeTransactionBody](#proto-FreezeTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_freeze-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/freeze.proto
# Freeze
Transaction body for a network &#34;freeze&#34; transaction.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-FreezeTransactionBody"></a>

### FreezeTransactionBody
A transaction body for all five freeze transactions.

Combining five different transactions into a single message, this
transaction body MUST support options to schedule a freeze, abort a
scheduled freeze, prepare a software upgrade, prepare a telemetry
upgrade, or initiate a software upgrade.

For a scheduled freeze, at the scheduled time, according to
network consensus time
  - A freeze (`FREEZE_ONLY`) causes the network nodes to stop creating
    events or accepting transactions, and enter a persistent
    maintenance state.
  - A freeze upgrade (`FREEZE_UPGRADE`) causes the network nodes to stop
    creating events or accepting transactions, and upgrade the node software
    from a previously prepared upgrade package. The network nodes then
    restart and rejoin the network after upgrading.

For other freeze types, immediately upon processing the freeze transaction
  - A Freeze Abort (`FREEZE_ABORT`) cancels any pending scheduled freeze.
  - A prepare upgrade (`PREPARE_UPGRADE`) begins to extract the contents of
    the specified upgrade file to the local filesystem.
  - A telemetry upgrade (`TELEMETRY_UPGRADE`) causes the network nodes to
    extract a telemetry upgrade package to the local filesystem and signal
    other software on the machine to upgrade, without impacting the node or
    network processing.

### Block Stream Effects
Unknown


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| startHour | [int32](#int32) |  | **Deprecated.** Rejected if set; replace with `start_time`.&lt;br/&gt; The start hour (in UTC time), a value between 0 and 23 |
| startMin | [int32](#int32) |  | **Deprecated.** Rejected if set; replace with `start_time`.&lt;br/&gt; The start minute (in UTC time), a value between 0 and 59 |
| endHour | [int32](#int32) |  | **Deprecated.** Rejected if set; end time is neither assigned nor guaranteed and depends on many uncontrolled factors.&lt;br/&gt; The end hour (in UTC time), a value between 0 and 23 |
| endMin | [int32](#int32) |  | **Deprecated.** Rejected if set; end time is neither assigned nor guaranteed and depends on many uncontrolled factors.&lt;br/&gt; The end minute (in UTC time), a value between 0 and 59 |
| update_file | [FileID](#proto-FileID) |  | An upgrade file. &lt;p&gt; If set, the identifier of a file in network state.&lt;br/&gt; The contents of this file MUST be a `zip` file and this data SHALL be extracted to the node filesystem during a `PREPARE_UPGRADE` or `TELEMETRY_UPGRADE` freeze type.&lt;br/&gt; The `file_hash` field MUST match the SHA384 hash of the content of this file.&lt;br/&gt; The extracted data SHALL be used to perform a network software update if a `FREEZE_UPGRADE` freeze type is subsequently processed. |
| file_hash | [bytes](#bytes) |  | A SHA384 hash of file content.&lt;br/&gt; This is a hash of the file identified by `update_file`. &lt;p&gt; This MUST be set if `update_file` is set, and MUST match the SHA384 hash of the contents of that file. |
| start_time | [Timestamp](#proto-Timestamp) |  | A start time for the freeze. &lt;p&gt; If this field is REQUIRED for the specified `freeze_type`, then when the network consensus time reaches this instant&lt;ol&gt; &lt;li&gt;The network SHALL stop accepting transactions.&lt;/li&gt; &lt;li&gt;The network SHALL gossip a freeze state.&lt;/li&gt; &lt;li&gt;The nodes SHALL, in coordinated order, disconnect and shut down.&lt;/li&gt; &lt;li&gt;The nodes SHALL halt or perform a software upgrade, depending on `freeze_type`.&lt;/li&gt; &lt;li&gt;If the `freeze_type` is `FREEZE_UPGRADE`, the nodes SHALL restart and rejoin the network upon completion of the software upgrade.&lt;/li&gt; &lt;/ol&gt; &lt;blockquote&gt; If the `freeze_type` is `TELEMETRY_UPGRADE`, the start time is required, but the network SHALL NOT stop, halt, or interrupt transaction processing. The required field is an historical anomaly and SHOULD change in a future release.&lt;/blockquote&gt; |
| freeze_type | [FreezeType](#proto-FreezeType) |  | The type of freeze. &lt;p&gt; This REQUIRED field effectively selects between five quite different transactions in the same transaction body. Depending on this value the service may schedule a freeze, prepare upgrades, perform upgrades, or even abort a previously scheduled freeze. |





 

 

 

 



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

