# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/file_delete.proto](#services_file_delete-proto)
    - [FileDeleteTransactionBody](#proto-FileDeleteTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_file_delete-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/file_delete.proto
# File Delete
A message for a transaction to delete a file.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-FileDeleteTransactionBody"></a>

### FileDeleteTransactionBody
Mark a file as deleted and remove its content from network state.

The metadata for a deleted file SHALL be retained at least until the
expiration time for the file is exceeded.&lt;br/&gt;
On completion, the identified file SHALL be marked `deleted`.&lt;br/&gt;
On completion, the identified file SHALL have an empty `contents` array.&lt;br/&gt;
This transaction SHALL be final and irreversible.&lt;br/&gt;

#### Signature Requirements
At least _one_ key from the `KeyList` in the `keys` field of the
identified file MUST sign this transaction.&lt;br/&gt;
If the keys field for the identified file is an empty `KeyList` (because that
file was previously created or updated to have an empty `KeyList`), then the
file is considered immutable and this message SHALL fail as UNAUTHORIZED.
See the [File Service](#FileService) specification for a detailed
explanation of the signature requirements for all file transactions.

### What is a &#34;system&#34; file
A &#34;system&#34; file is any file with a file number less than or equal to the
current configuration value for `ledger.numReservedSystemEntities`,
typically `750`.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fileID | [FileID](#proto-FileID) |  | A file identifier.&lt;br/&gt; This identifies the file to delete. &lt;p&gt; The identified file MUST NOT be a &#34;system&#34; file.&lt;br/&gt; This field is REQUIRED. |





 

 

 

 



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

