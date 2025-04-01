# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [streams/contract_action.proto](#streams_contract_action-proto)
    - [ContractAction](#proto-ContractAction)
    - [ContractActions](#proto-ContractActions)
  
    - [CallOperationType](#proto-CallOperationType)
    - [ContractActionType](#proto-ContractActionType)
  
- [Scalar Value Types](#scalar-value-types)



<a name="streams_contract_action-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## streams/contract_action.proto



<a name="proto-ContractAction"></a>

### ContractAction
A finer grained action with a function result. Sometimes called &#34;internal transactions.&#34; The function call itself
will be the first action in a list, followed by sub-action in the order they were executed.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| call_type | [ContractActionType](#proto-ContractActionType) |  | The type of this action. |
| calling_account | [AccountID](#proto-AccountID) |  | If the caller was a regular account, the AccountID. |
| calling_contract | [ContractID](#proto-ContractID) |  | If the caller was a smart contract account, the ContractID. |
| gas | [int64](#int64) |  | The upper limit of gas this action can spend. |
| input | [bytes](#bytes) |  | Bytes passed in as input data to this action. |
| recipient_account | [AccountID](#proto-AccountID) |  | The AccountID of the recipient if the recipient is an account. Only HBars will be transferred, no other side effects should be expected. |
| recipient_contract | [ContractID](#proto-ContractID) |  | The ContractID of the recipient if the recipient is a smart contract. |
| targeted_address | [bytes](#bytes) |  | The bytes of the targeted by the action address. Only set on failed executions. If set, denotes that the address did not correspond to any account or contract at the time of finalization of this action. An example would be a failed lazy create as per HIP-583. |
| value | [int64](#int64) |  | The value (in tinybars) that is associated with this action. |
| gas_used | [int64](#int64) |  | The actual gas spent by this action. |
| output | [bytes](#bytes) |  | If successful, the output bytes of the action. |
| revert_reason | [bytes](#bytes) |  | The contract itself caused the transaction to fail via the `REVERT` operation |
| error | [bytes](#bytes) |  | The transaction itself failed without an explicit `REVERT` |
| call_depth | [int32](#int32) |  | The nesting depth of this call. The original action is at depth=0. |
| call_operation_type | [CallOperationType](#proto-CallOperationType) |  | The call operation type |






<a name="proto-ContractActions"></a>

### ContractActions



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contract_actions | [ContractAction](#proto-ContractAction) | repeated |  |





 


<a name="proto-CallOperationType"></a>

### CallOperationType
The specific operation type of a call. The OP prefix has been added to avoid name collisions for
the CALL and CREATE operation types since both ContractActionType and CallOperationType enums are
used in ContractAction

| Name | Number | Description |
| ---- | ------ | ----------- |
| OP_UNKNOWN | 0 | default operation type is UNKNOWN |
| OP_CALL | 1 | CALL operation type. |
| OP_CALLCODE | 2 | CALLCODE operation type |
| OP_DELEGATECALL | 3 | DELEGATECALL operation type |
| OP_STATICCALL | 4 | STATICCALL operation type |
| OP_CREATE | 5 | CREATE operation type |
| OP_CREATE2 | 6 | CREATE2 operation type |



<a name="proto-ContractActionType"></a>

### ContractActionType
The type of action described by the action proto.

| Name | Number | Description |
| ---- | ------ | ----------- |
| NO_ACTION | 0 | default non-value. |
| CALL | 1 | Most CALL, CALLCODE, DELEGATECALL, and STATICCALL, and first action of ContractCall/ContractCallLocal to deployed contracts. This does not include calls to system or precompiled contracts. |
| CREATE | 2 |  |
| PRECOMPILE | 3 | like Call, but to precompiled contracts (0x1 to 0x9 as of Berlin) |
| SYSTEM | 4 | Call, but to system contract like HTS or ERC20 facades over Token accounts |


 

 

 



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

