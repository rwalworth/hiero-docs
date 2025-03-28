# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/crypto_get_account_balance.proto](#services_crypto_get_account_balance-proto)
    - [CryptoGetAccountBalanceQuery](#proto-CryptoGetAccountBalanceQuery)
    - [CryptoGetAccountBalanceResponse](#proto-CryptoGetAccountBalanceResponse)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_crypto_get_account_balance-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/crypto_get_account_balance.proto
# Crypto Get Account Balance
Query request to obtain balance information for a single account.

This query SHOULD NOT be used by client software, queries to a
Mirror Node provide more information at much lower cost.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-CryptoGetAccountBalanceQuery"></a>

### CryptoGetAccountBalanceQuery
Query to read the HBAR balance of an account or contract.

This query SHALL return _only_ the HBAR balance for an account
or smart contract. Early releases of the network would return all
fungible/common token balances, but HIP-367 made it infeasible to
return all such balances. This query SHALL NOT return any information
beyond the current HBAR balance.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| accountID | [AccountID](#proto-AccountID) |  | An account identifier.&lt;br/&gt; This identifies an account for which the balance is requested. &lt;p&gt; Exactly one identifier MUST be provided. |
| contractID | [ContractID](#proto-ContractID) |  | A smart contract identifier.&lt;br/&gt; This identifies a smart contract for which the balance is requested. &lt;p&gt; Exactly one identifier MUST be provided. |






<a name="proto-CryptoGetAccountBalanceResponse"></a>

### CryptoGetAccountBalanceResponse
Response to a CryptoGetAccountBalanceQuery.&lt;br/&gt;

This response SHALL contain only the information needed to
identify the query request and the actual HBAR balance of the
identified account or contract.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| accountID | [AccountID](#proto-AccountID) |  | An account identifier.&lt;br/&gt; This is the account ID queried. &lt;br/&gt; The inclusion of the account queried is useful with state proofs, when needed to prove an account balance to a third party. |
| balance | [uint64](#uint64) |  | A current account balance.&lt;br/&gt; This is the current HBAR balance denominated in tinybar (10&lt;sup&gt;-8&lt;/sup&gt; HBAR). |
| tokenBalances | [TokenBalance](#proto-TokenBalance) | repeated | **Deprecated.** This field became infeasible to support after HIP-367 removed limits on the number of associated tokens.&lt;br/&gt; A list of token balances for all tokens associated to the account. &lt;p&gt; This field was deprecated by &lt;a href=&#34;https://hips.hedera.com/hip/hip-367&#34;&gt;HIP-367&lt;/a&gt;, which allowed an account to be associated to an unlimited number of tokens. This scale makes it more efficient for users to consult mirror nodes to review their token balances. |





 

 

 

 



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

