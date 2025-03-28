# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/get_by_solidity_id.proto](#services_get_by_solidity_id-proto)
    - [GetBySolidityIDQuery](#proto-GetBySolidityIDQuery)
    - [GetBySolidityIDResponse](#proto-GetBySolidityIDResponse)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_get_by_solidity_id-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/get_by_solidity_id.proto
# Get By Solidity
A standard query to obtain account and contract identifiers for a smart
contract, given the Solidity identifier for that contract.

&gt; Important
&gt;&gt; This query is obsolete and not supported.&lt;br/&gt;
&gt;&gt; Any query of this type that is submitted SHALL fail with a `PRE_CHECK`
&gt;&gt; result of `NOT_SUPPORTED`.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-GetBySolidityIDQuery"></a>

### GetBySolidityIDQuery
Query to read Contract, Account, and File identifiers for a smart
contract given a Solidity identifier.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| solidityID | [string](#string) |  | A contract ID in the format used by Solidity. &lt;p&gt; This field is REQUIRED. |






<a name="proto-GetBySolidityIDResponse"></a>

### GetBySolidityIDResponse
Response to a getBySolidityId query.

This message returns the account, contract, and file identifiers for
a smart contract.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| accountID | [AccountID](#proto-AccountID) |  | An account identifier. &lt;p&gt; This SHALL identify an account that backs the requested smart contract. |
| fileID | [FileID](#proto-FileID) |  | A file identifier. &lt;p&gt; This SHALL identify a file, the contents of which are the EVM bytecode for the requested smart contract. |
| contractID | [ContractID](#proto-ContractID) |  | A contract identifier. &lt;p&gt; This SHALL identify the requested smart contract. |





 

 

 

 



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

