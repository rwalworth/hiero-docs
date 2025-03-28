# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [streams/account_balance_file.proto](#streams_account_balance_file-proto)
    - [AllAccountBalances](#proto-AllAccountBalances)
    - [SingleAccountBalances](#proto-SingleAccountBalances)
    - [TokenUnitBalance](#proto-TokenUnitBalance)
  
- [Scalar Value Types](#scalar-value-types)



<a name="streams_account_balance_file-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## streams/account_balance_file.proto



<a name="proto-AllAccountBalances"></a>

### AllAccountBalances
Includes all currency balances (both hbar and token) of all accounts in the ledger.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| consensusTimestamp | [Timestamp](#proto-Timestamp) |  | An instant in consensus time |
| allAccounts | [SingleAccountBalances](#proto-SingleAccountBalances) | repeated | The list of account balances for all accounts, after handling all transactions with consensus timestamp up to and including the above instant |






<a name="proto-SingleAccountBalances"></a>

### SingleAccountBalances
Includes all currency balances (both hbar and token) of a single account in the ledger.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accountID | [AccountID](#proto-AccountID) |  | The account |
| hbarBalance | [uint64](#uint64) |  | The account&#39;s hbar balance |
| tokenUnitBalances | [TokenUnitBalance](#proto-TokenUnitBalance) | repeated | The list of the account&#39;s token balances |






<a name="proto-TokenUnitBalance"></a>

### TokenUnitBalance



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tokenId | [TokenID](#proto-TokenID) |  | A unique token id |
| balance | [uint64](#uint64) |  | Number of transferable units of the identified token. For token of type FUNGIBLE_COMMON - balance in the smallest denomination. For token of type NON_FUNGIBLE_UNIQUE - the number of NFTs held by the account |





 

 

 

 



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

