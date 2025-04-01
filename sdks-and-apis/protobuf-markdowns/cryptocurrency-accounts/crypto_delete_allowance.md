# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/crypto_delete_allowance.proto](#services_crypto_delete_allowance-proto)
    - [CryptoDeleteAllowanceTransactionBody](#proto-CryptoDeleteAllowanceTransactionBody)
    - [NftRemoveAllowance](#proto-NftRemoveAllowance)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_crypto_delete_allowance-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/crypto_delete_allowance.proto
# Crypto Delete Allowance
Delete one or more NFT allowances that permit transfer of tokens from
an &#34;owner&#34; account by a different, &#34;spender&#34;, account.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-CryptoDeleteAllowanceTransactionBody"></a>

### CryptoDeleteAllowanceTransactionBody
Delete one or more allowances.&lt;br/&gt;
Given one or more, previously approved, allowances for non-fungible/unique
tokens to be transferred by a spending account from an owning account;
this transaction removes a specified set of those allowances.

The owner account for each listed allowance MUST sign this transaction.&lt;br/&gt;
Allowances for HBAR cannot be removed with this transaction. The owner
account MUST submit a new `cryptoApproveAllowance` transaction with the
amount set to `0` to &#34;remove&#34; that allowance.&lt;br/&gt;
Allowances for fungible/common tokens cannot be removed with this
transaction. The owner account MUST submit a new `cryptoApproveAllowance`
transaction with the amount set to `0` to &#34;remove&#34; that allowance.&lt;br/&gt;

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| nftAllowances | [NftRemoveAllowance](#proto-NftRemoveAllowance) | repeated | List of non-fungible/unique token allowances to remove. &lt;p&gt; This list MUST NOT be empty. |






<a name="proto-NftRemoveAllowance"></a>

### NftRemoveAllowance
A single allowance for one non-fungible/unique token.
This is specific to removal, and the allowance is identified for that
specific purpose.

All fields in this message are REQUIRED.
The `serial_numbers` list MUST NOT be empty.
The combination of field values in this message MUST match existing
allowances for one or more individual non-fungible/unique tokens.

### Removing an allowance that is `approve_for_all`
To remove an allowance that has set the `approve_for_all` flag, the
`owner` account must first _approve_ a **new** allowance for a specific
serial number using a `cryptoApproveAllowance`, and then, if desired,
that newly approved allowance to a specific serial number may be
deleted in a separate `cryptoDeleteAllowance` transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_id | [TokenID](#proto-TokenID) |  | A token identifier. &lt;p&gt; This MUST be a valid token identifier for a non-fungible/unique token type. |
| owner | [AccountID](#proto-AccountID) |  | An `owner` account identifier. &lt;p&gt; This account MUST sign the transaction containing this message. |
| serial_numbers | [int64](#int64) | repeated | The list of serial numbers to remove allowances from. &lt;p&gt; This list MUST NOT be empty. |





 

 

 

 



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

