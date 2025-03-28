# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/node_delete.proto](#services_node_delete-proto)
    - [NodeDeleteTransactionBody](#com-hedera-hapi-node-addressbook-NodeDeleteTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_node_delete-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/node_delete.proto



<a name="com-hedera-hapi-node-addressbook-NodeDeleteTransactionBody"></a>

### NodeDeleteTransactionBody
A transaction body to delete a node from the network address book.

- A `NodeDeleteTransactionBody` MUST be signed by one of those keys:
  adminKey, treasure account (2) key, systemAdmin(50) key, or
  addressBookAdmin(55) key.
- Upon success, the address book entry SHALL enter a &#34;pending delete&#34;
  state.
- All address book entries pending deletion SHALL be removed from the
  active network configuration during the next `freeze` transaction with
  the field `freeze_type` set to `PREPARE_UPGRADE`.&lt;br/&gt;
- A deleted address book node SHALL be removed entirely from network state.
- A deleted address book node identifier SHALL NOT be reused.

### Block Stream Effects
None.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | A consensus node identifier in the network state. &lt;p&gt; The node identified MUST exist in the network address book.&lt;br/&gt; The node identified MUST NOT be deleted.&lt;br/&gt; This value is REQUIRED. |





 

 

 

 



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

