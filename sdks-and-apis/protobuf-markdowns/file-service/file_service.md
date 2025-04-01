# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/output/file_service.proto](#block_stream_output_file_service-proto)
    - [AppendFileOutput](#com-hedera-hapi-block-stream-output-AppendFileOutput)
    - [CreateFileOutput](#com-hedera-hapi-block-stream-output-CreateFileOutput)
    - [DeleteFileOutput](#com-hedera-hapi-block-stream-output-DeleteFileOutput)
    - [SystemDeleteOutput](#com-hedera-hapi-block-stream-output-SystemDeleteOutput)
    - [SystemUndeleteOutput](#com-hedera-hapi-block-stream-output-SystemUndeleteOutput)
    - [UpdateFileOutput](#com-hedera-hapi-block-stream-output-UpdateFileOutput)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_output_file_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/output/file_service.proto
# File Service
Block stream messages that report the results of transactions handled
by the `File` service.

A `file` in Hedera is an arbitrary sequence of bytes and may be up to
`1048576` total bytes. Files are used anywhere a transaction requires a
large amount of data (anything that would not fit within the
transaction size limit).

Examples
 - smart contract bytecode
 - network configuration updates
 - network fee schedules
 - image files for NFTs
 - property title documents
There are many other uses; these examples are illustrative.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-output-AppendFileOutput"></a>

### AppendFileOutput
Block Stream data for a `fileAppend` transaction.

This message SHALL NOT duplicate information already contained
in the original transaction.






<a name="com-hedera-hapi-block-stream-output-CreateFileOutput"></a>

### CreateFileOutput
Block Stream data for a `fileCreate` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-DeleteFileOutput"></a>

### DeleteFileOutput
Block Stream data for a `fileDelete` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-SystemDeleteOutput"></a>

### SystemDeleteOutput
Block Stream data for a `systemDelete` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-SystemUndeleteOutput"></a>

### SystemUndeleteOutput
Block Stream data for a `systemUndelete` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-UpdateFileOutput"></a>

### UpdateFileOutput
Block Stream data for a `fileUpdate` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.





 

 

 

 



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

