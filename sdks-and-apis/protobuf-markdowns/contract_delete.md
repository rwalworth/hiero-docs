# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/contract_delete.proto](#services_contract_delete-proto)
    - [ContractDeleteTransactionBody](#proto-ContractDeleteTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_contract_delete-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/contract_delete.proto
# Contract Delete
Delete a smart contract, transferring any remaining balance to a
designated account.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ContractDeleteTransactionBody"></a>

### ContractDeleteTransactionBody
Delete a smart contract, and transfer any remaining HBAR balance to a
designated account.

If this call succeeds then all subsequent calls to that smart contract
SHALL execute the `0x0` opcode, as required for EVM equivalence.

### Requirements
 - An account or smart contract MUST be designated to receive all remaining
   account balances.
 - The smart contract MUST have an admin key set. If the contract does not
   have `admin_key` set, then this transaction SHALL fail and response code
   `MODIFYING_IMMUTABLE_CONTRACT` SHALL be set.
 - If `admin_key` is, or contains, an empty `KeyList` key, it SHALL be
   treated the same as an admin key that is not set.
 - The `Key` set for `admin_key` on the smart contract MUST have a valid
   signature set on this transaction.
 - The designated receiving account MAY have `receiver_sig_required` set. If
   that field is set, the receiver account MUST also sign this transaction.
 - The field `permanent_removal` MUST NOT be set. That field is reserved for
   internal system use when purging the smart contract from state. Any user
   transaction with that field set SHALL be rejected and a response code
   `PERMANENT_REMOVAL_REQUIRES_SYSTEM_INITIATION` SHALL be set.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractID | [ContractID](#proto-ContractID) |  | The id of the contract to be deleted. &lt;p&gt; This field is REQUIRED. |
| transferAccountID | [AccountID](#proto-AccountID) |  | An Account ID recipient. &lt;p&gt; This account SHALL receive all HBAR and other tokens still owned by the contract that is removed. |
| transferContractID | [ContractID](#proto-ContractID) |  | A contract ID recipient. &lt;p&gt; This contract SHALL receive all HBAR and other tokens still owned by the contract that is removed. |
| permanent_removal | [bool](#bool) |  | A flag indicating that this transaction is &#34;synthetic&#34;; initiated by the node software. &lt;p&gt; The consensus nodes create such &#34;synthetic&#34; transactions to both to properly manage state changes and to communicate those changes to other systems via the Block Stream.&lt;br/&gt; A user-initiated transaction MUST NOT set this flag. |





 

 

 

 



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

