# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/file_append.proto](#services_file_append-proto)
    - [FileAppendTransactionBody](#proto-FileAppendTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_file_append-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/file_append.proto
# File Append
A transaction body message to append data to a &#34;file&#34; in state.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-FileAppendTransactionBody"></a>

### FileAppendTransactionBody
A transaction body for an `appendContent` transaction.&lt;br/&gt;
This transaction body provides a mechanism to append content to a &#34;file&#34; in
network state. Hedera transactions are limited in size, but there are many
uses for in-state byte arrays (e.g. smart contract bytecode) which require
more than may fit within a single transaction. The `appendFile` transaction
exists to support these requirements. The typical pattern is to create a
file, append more data until the full content is stored, verify the file is
correct, then update the file entry with any final metadata changes (e.g.
adding threshold keys and removing the initial upload key).

Each append transaction MUST remain within the total transaction size limit
for the network (typically 6144 bytes).&lt;br/&gt;
The total size of a file MUST remain within the maximum file size limit for
the network (typically 1048576 bytes).

#### Signature Requirements
Append transactions MUST have signatures from _all_ keys in the `KeyList`
assigned to the `keys` field of the file.&lt;br/&gt;
See the [File Service](#FileService) specification for a detailed
explanation of the signature requirements for all file transactions.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fileID | [FileID](#proto-FileID) |  | A file identifier.&lt;br/&gt; This identifies the file to which the `contents` will be appended. &lt;p&gt; This field is REQUIRED.&lt;br/&gt; The identified file MUST exist.&lt;br/&gt; The identified file MUST NOT be larger than the current maximum file size limit.&lt;br/&gt; The identified file MUST NOT be deleted.&lt;br/&gt; The identified file MUST NOT be immutable. |
| contents | [bytes](#bytes) |  | An array of bytes to append.&lt;br/&gt; &lt;p&gt; This content SHALL be appended to the identified file if this transaction succeeds.&lt;br/&gt; This field is REQUIRED.&lt;br/&gt; This field MUST NOT be empty. |





 

 

 

 



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

