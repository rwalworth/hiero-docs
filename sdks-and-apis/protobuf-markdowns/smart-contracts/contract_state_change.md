# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [streams/contract_state_change.proto](#streams_contract_state_change-proto)
    - [ContractStateChange](#proto-ContractStateChange)
    - [ContractStateChanges](#proto-ContractStateChanges)
    - [StorageChange](#proto-StorageChange)
  
- [Scalar Value Types](#scalar-value-types)



<a name="streams_contract_state_change-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## streams/contract_state_change.proto



<a name="proto-ContractStateChange"></a>

### ContractStateChange
The storage changes to a smart contract&#39;s storage as a side effect of the function call.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contract_id | [ContractID](#proto-ContractID) |  | The contract to which the storage changes apply to |
| storage_changes | [StorageChange](#proto-StorageChange) | repeated | The list of storage changes. |






<a name="proto-ContractStateChanges"></a>

### ContractStateChanges



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contract_state_changes | [ContractStateChange](#proto-ContractStateChange) | repeated |  |






<a name="proto-StorageChange"></a>

### StorageChange
A storage slot change description.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| slot | [bytes](#bytes) |  | The storage slot changed. Up to 32 bytes, big-endian, zero bytes left trimmed. |
| value_read | [bytes](#bytes) |  | The value read from the storage slot. Up to 32 bytes, big-endian, zero bytes left trimmed.

Because of the way SSTORE operations are charged the slot is always read before being written to. |
| value_written | [google.protobuf.BytesValue](#google-protobuf-BytesValue) |  | The new value written to the slot. Up to 32 bytes, big-endian, zero bytes left trimmed.

If a value of zero is written the valueWritten will be present but the inner value will be absent.

If a value was read and not written this value will not be present. |





 

 

 

 



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

