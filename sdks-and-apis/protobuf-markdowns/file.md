# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/file/file.proto](#services_state_file_file-proto)
    - [File](#proto-File)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_file_file-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/file/file.proto
# File
A &#34;file&#34; in the distributed ledger is a stream of bytes. These bytes may
contain any data, and are limited in length based on network configuration
(for example, 1048576).


### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-File"></a>

### File
Representation of an Hedera File Service `file`.

Files offer a place to store additional data, much more than is available in
other entities, for use with smart contracts, non-fungible tokens, etc...
As with all network entities, a file has a unique entity number, which is
given along with the network&#39;s shard and realm in the form of a
shard.realm.number id.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| file_id | [FileID](#proto-FileID) |  | This file&#39;s ID within the global network state. &lt;p&gt; This value SHALL be unique within the network. |
| expiration_second | [int64](#int64) |  | The file&#39;s expiration time in seconds since the epoch.&lt;br/&gt; This value should be compared against consensus time, which may not exactly match clock time at the moment of expiration. &lt;p&gt; For this purpose, `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`. |
| keys | [KeyList](#proto-KeyList) |  | A list of keys that MUST sign any transaction to create or update this file. &lt;p&gt; Only _one_ of these keys must sign a transaction to delete the file.&lt;br/&gt; This field MAY be `null` or an empty list.&lt;br/&gt; If this field is null or an empty `KeyList`, then the file SHALL be immutable.&lt;br/&gt; For an immutable file, the only transaction permitted to modify that file SHALL be a `fileUpdate` transaction with _only_ the `expirationTime` set. |
| contents | [bytes](#bytes) |  | The contents of the file. &lt;p&gt; This SHALL be limited to the current maximum file size; typically no more than 1 Megabyte (1048576 bytes). |
| memo | [string](#string) |  | A short description of the file. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| deleted | [bool](#bool) |  | A flag indicating that this file is deleted. &lt;p&gt; The `contents` of a deleted &#34;regular&#34; file SHALL be an empty (zero length) bytes. |
| pre_system_delete_expiration_second | [int64](#int64) |  | The pre-system-delete expiration time of a deleted &#34;system&#34; file, in seconds. &lt;p&gt; This field SHALL contain the original expiration time of a &#34;system&#34; file that is deleted. This SHOULD be used to restore that expiration time if the file is subsequently &#34;un-deleted&#34; before it is purged from the system.&lt;br/&gt; A &#34;regular&#34; file cannot be &#34;un-deleted&#34;, so this field SHALL NOT be set for those files. |





 

 

 

 



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

