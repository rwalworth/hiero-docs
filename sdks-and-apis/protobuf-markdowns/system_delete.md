# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/system_delete.proto](#services_system_delete-proto)
    - [SystemDeleteTransactionBody](#proto-SystemDeleteTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_system_delete-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/system_delete.proto
# System Delete
A system transaction to remove a file from the Hedera File
Service (HFS).&lt;br/&gt;
This transaction is a privileged operation restricted to &#34;system&#34;
accounts.

&gt; Note
&gt;&gt; System delete is defined here for a smart contract (to delete
&gt;&gt; the bytecode), but was never implemented.
&gt;
&gt;&gt; Currently, system delete and system undelete specifying a smart
&gt;&gt; contract identifier SHALL return `INVALID_FILE_ID`
&gt;&gt; or `MISSING_ENTITY_ID`.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-SystemDeleteTransactionBody"></a>

### SystemDeleteTransactionBody
Delete a file or contract bytecode as an administrative transaction.

&gt; Note
&gt;&gt; A system delete/undelete for a `contractID` is not supported and
&gt;&gt; SHALL return `INVALID_FILE_ID` or `MISSING_ENTITY_ID`.

This transaction MAY be reversed by the `systemUndelete` transaction.
A file deleted via `fileDelete`, however SHALL be irrecoverable.&lt;br/&gt;
This transaction MUST specify an expiration timestamp (with seconds
precision). The file SHALL be permanently removed from state when
network consensus time exceeds the specified expiration time.&lt;br/&gt;
This transaction MUST be signed by an Hedera administrative (&#34;system&#34;)
account.

### What is a &#34;system&#34; file
A &#34;system&#34; file is any file with a file number less than or equal to the
current configuration value for `ledger.numReservedSystemEntities`,
typically `750`.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fileID | [FileID](#proto-FileID) |  | A file identifier. &lt;p&gt; The identified file MUST exist in the HFS.&lt;br/&gt; The identified file MUST NOT be deleted.&lt;br/&gt; The identified file MUST NOT be a &#34;system&#34; file.&lt;br/&gt; This field is REQUIRED. |
| contractID | [ContractID](#proto-ContractID) |  | A contract identifier. &lt;p&gt; The identified contract MUST exist in network state.&lt;br/&gt; The identified contract bytecode MUST NOT be deleted.&lt;br/&gt; &lt;p&gt; This option is _unsupported_. |
| expirationTime | [TimestampSeconds](#proto-TimestampSeconds) |  | A timestamp indicating when the file will be removed from state. &lt;p&gt; This value SHALL be expressed in seconds since the `epoch`. The `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`.&lt;br/&gt; This field is REQUIRED. |





 

 

 

 



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

