# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/contract/storage_slot.proto](#services_state_contract_storage_slot-proto)
    - [SlotKey](#proto-SlotKey)
    - [SlotValue](#proto-SlotValue)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_contract_storage_slot-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/contract/storage_slot.proto
# EVM Contract Slot
Information regarding EVM storage &#34;slot&#34;s for the
Hedera Smart Contract service.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-SlotKey"></a>

### SlotKey
The key of a storage slot. A slot is scoped to a specific contract ID.

For each contract, its EVM storage is a mapping of 256-bit keys (or &#34;words&#34;)
to 256-bit values.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractID | [ContractID](#proto-ContractID) |  | The Contract ID of the contract that owns (and pays for) this slot. |
| key | [bytes](#bytes) |  | The EVM key of this slot, left-padded with zeros to form a 256-bit word. |






<a name="proto-SlotValue"></a>

### SlotValue
The value of a contract storage slot. For the EVM, this is a single &#34;word&#34;.

Because we iterate through all the storage slots for an expired contract
when purging it from state, our slot values also include the words of the
previous and next keys in this contract&#39;s storage &#34;virtual linked list&#34;.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| value | [bytes](#bytes) |  | The EVM value in this slot, left-padded with zeros to form a 256-bit word. |
| previous_key | [bytes](#bytes) |  | The word of the previous key in this contract&#39;s storage list (if any). |
| next_key | [bytes](#bytes) |  | The word of the next key in this contract&#39;s storage list (if any). |





 

 

 

 



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

