# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/file_create.proto](#services_file_create-proto)
    - [FileCreateTransactionBody](#proto-FileCreateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_file_create-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/file_create.proto
# File Create
Messages to create a new file entry.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-FileCreateTransactionBody"></a>

### FileCreateTransactionBody
Create a new file.

If successful, the new file SHALL contain the (possibly empty) content
provided in the `contents` field.&lt;br/&gt;
When the current consensus time exceeds the `expirationTime` value, the
network SHALL expire the file, and MAY archive the state entry.

#### Signature Requirements
The HFS manages file authorization in a manner that can be confusing.
The core element of file authorization is the `keys` field,
which is a `KeyList`; a list of individual `Key` messages, each of which
may represent a simple or complex key.&lt;br/&gt;
The file service transactions treat this list differently.&lt;br/&gt;
A `fileCreate`, `fileAppend`, or `fileUpdate` MUST have a valid signature
from _each_ key in the list.&lt;br/&gt;
A `fileDelete` MUST have a valid signature from _at least one_ key in
the list. This is different, and allows a file &#34;owned&#34; by many entities
to be deleted by any one of those entities. A deleted file cannot be
restored, so it is important to consider this when assigning keys for
a file.&lt;br/&gt;
If any of the keys in a `KeyList` are complex, the full requirements of
each complex key must be met to count as a &#34;valid signature&#34; for that key.
A complex key structure (i.e. a `ThresholdKey`, or `KeyList`, possibly
including additional `ThresholdKey` or `KeyList` descendants) may be
assigned as the sole entry in a file `keys` field to ensure all transactions
have the same signature requirements.

If the `keys` field is an empty `KeyList`, then the file SHALL be immutable
and the only transaction permitted to modify that file SHALL be a
`fileUpdate` transaction with _only_ the `expirationTime` set.

#### Shard and Realm
The current API ignores shardID and realmID. All files are created in
shard 0 and realm 0. Future versions of the API may support multiple
realms and multiple shards.

### Block Stream Effects
After the file is created, the FileID for it SHALL be returned in the
transaction receipt, and SHALL be recorded in the transaction record.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| expirationTime | [Timestamp](#proto-Timestamp) |  | An expiration timestamp. &lt;p&gt; When the network consensus time exceeds this value, the network SHALL expire the file. |
| keys | [KeyList](#proto-KeyList) |  | A list of keys that represent file &#34;owners&#34;. &lt;p&gt; Every `Key` in this list MUST sign this `fileCreate` transaction, as well as any `fileUpdate` or `fileAppend` that modifies this file.&lt;br/&gt; At least one `Key` in this list MUST sign any `fileDelete` transaction to delete this file.&lt;br/&gt; If this `KeyList` is empty, the file SHALL be created immutable and the only field that may be changed subsequently is the `expirationTime`. An immutable file cannot be deleted except with a `systemDelete` transaction, or by expiration. |
| contents | [bytes](#bytes) |  | A byte array of file content. &lt;p&gt; The file SHALL be created with initial content equal to this field. |
| shardID | [ShardID](#proto-ShardID) |  | A shard in which this file is created |
| realmID | [RealmID](#proto-RealmID) |  | A realm in which this file is created. &lt;p&gt; The shard number for this realm MUST match the value in `shardID`.&lt;br/&gt; Currently, this MUST be `0` for both fields.&lt;br/&gt; If the desired realm is `0.0`, this SHOULD NOT be set. |
| newRealmAdminKey | [Key](#proto-Key) |  | **Deprecated.** The &#34;create realm&#34; was never enabled, and should not be possible on file creation.&lt;br/&gt; An admin key for a new realm, if one is created. Added deprecated tag 2024-05. |
| memo | [string](#string) |  | A short description of this file. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |





 

 

 

 



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

