# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/contract_call.proto](#services_contract_call-proto)
    - [ContractCallTransactionBody](#proto-ContractCallTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_contract_call-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/contract_call.proto
# Contract Call
Transaction body for calls to a Smart Contract.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ContractCallTransactionBody"></a>

### ContractCallTransactionBody
Call a function of a given smart contract, providing function parameter
inputs as needed.

Resource (&#34;gas&#34;) charges SHALL include all relevant fees incurred by the
contract execution, including any storage required.&lt;br/&gt;
The total transaction fee SHALL incorporate all of the &#34;gas&#34; actually
consumed as well as the standard fees for transaction handling, data
transfers, signature verification, etc...&lt;br/&gt;
The response SHALL contain the output returned by the function call.

### Block Stream Effects
A `CallContractOutput` message SHALL be emitted for each transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractID | [ContractID](#proto-ContractID) |  | The ID of a smart contract to call. |
| gas | [int64](#int64) |  | A maximum limit to the amount of gas to use for this call. &lt;p&gt; The network SHALL charge the greater of the following, but SHALL NOT charge more than the value of this field. &lt;ol&gt; &lt;li&gt;The actual gas consumed by the smart contract call.&lt;/li&gt; &lt;li&gt;`80%` of this value.&lt;/li&gt; &lt;/ol&gt; The `80%` factor encourages reasonable estimation, while allowing for some overage to ensure successful execution. |
| amount | [int64](#int64) |  | An amount of tinybar sent via this contract call. &lt;p&gt; If this is non-zero, the function MUST be `payable`. |
| functionParameters | [bytes](#bytes) |  | The smart contract function to call. &lt;p&gt; This MUST contain The application binary interface (ABI) encoding of the function call per the Ethereum contract ABI standard, giving the function signature and arguments being passed to the function. |





 

 

 

 



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

