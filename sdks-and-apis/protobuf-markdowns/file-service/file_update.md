# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/file_update.proto](#services_file_update-proto)
    - [FileUpdateTransactionBody](#proto-FileUpdateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_file_update-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/file_update.proto
# File Update
A message to modify the metadata for a file and/or _replace_ the contents.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-FileUpdateTransactionBody"></a>

### FileUpdateTransactionBody
Update the metadata, and/or replace the content, of a file in the
Hedera File Service (HFS).

Any field which is not set (i.e. is null) in this message, other than
`fileID`, SHALL be ignored.&lt;br/&gt;
If the `keys` list for the identified file is an empty `KeyList`, then
this message MUST NOT set any field except `expirationTime`.

#### Signature Requirements
Every `Key` in the `keys` list for the identified file MUST sign this
transaction, if any field other than `expirationTime` is to be updated.&lt;br/&gt;
If the `keys` list for the identified file is an empty `KeyList` (because
this file was previously created or updated to have an empty `KeyList`),
then the file is considered immutable and this message MUST NOT set any
field except `expirationTime`.&lt;br/&gt;
See the [File Service](#FileService) specification for a detailed
explanation of the signature requirements for all file transactions.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fileID | [FileID](#proto-FileID) |  | A file identifier for the file to update. &lt;p&gt; This field is REQUIRED. |
| expirationTime | [Timestamp](#proto-Timestamp) |  | An expiration timestamp. &lt;p&gt; If set, this value MUST be strictly later than the existing `expirationTime` value, or else it will be ignored.&lt;br/&gt; If set, this value SHALL replace the existing `expirationTime`.&lt;br/&gt; If this field is the only field set, then this transaction SHALL NOT require any signature other than the `payer` for the transaction.&lt;br/&gt; When the network consensus time exceeds the then-current `expirationTime`, the network SHALL expire the file. |
| keys | [KeyList](#proto-KeyList) |  | The new list of keys that &#34;own&#34; this file. &lt;p&gt; If set, every key in this `KeyList` MUST sign this transaction.&lt;br/&gt; If set, every key in the _previous_ `KeyList` MUST _also_ sign this transaction.&lt;br/&gt; If this value is an empty `KeyList`, then the file SHALL be immutable after completion of this transaction. |
| contents | [bytes](#bytes) |  | An array of bytes. &lt;p&gt; This value, if set, SHALL _replace_ the existing file content. If this value is set to an empty byte array, the content of the file SHALL be unchanged. |
| memo | [google.protobuf.StringValue](#google-protobuf-StringValue) |  | A short description of this file. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |





 

 

 

 



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

