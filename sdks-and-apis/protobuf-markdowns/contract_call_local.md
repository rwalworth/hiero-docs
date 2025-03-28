# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/contract_call_local.proto](#services_contract_call_local-proto)
    - [ContractCallLocalQuery](#proto-ContractCallLocalQuery)
    - [ContractCallLocalResponse](#proto-ContractCallLocalResponse)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_contract_call_local-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/contract_call_local.proto
# Local Contract Call
A Contract Call executed directly on the current node
(that is, without consensus).

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ContractCallLocalQuery"></a>

### ContractCallLocalQuery
Call a view function of a given smart contract&lt;br/&gt;
The call must provide function parameter inputs as needed.&lt;br/&gt;
This is potentially useful for calling view functions that will not revert
when executed in a static EVM context. Many such use cases will be better
served by using a Mirror Node API, however.

This is performed locally on the particular node that the client is
communicating with. Executing the call locally is faster and less costly,
but imposes certain restrictions.&lt;br/&gt;
The call MUST NOT change the state of the contract instance. This also
precludes any expenditure or transfer of HBAR or other tokens.&lt;br/&gt;
The call SHALL NOT have a separate consensus timestamp.&lt;br/&gt;
The call SHALL NOT generate a record nor a receipt.&lt;br/&gt;
The response SHALL contain the output returned by the function call.&lt;br/&gt;
Any contract call that would use the `STATICCALL` opcode MAY be called via
contract call local with performance and cost benefits.

Unlike a ContractCall transaction, the node SHALL always consume the
_entire_ amount of offered &#34;gas&#34; in determining the fee for this query, so
accurate gas estimation is important.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). &lt;p&gt; The payment MUST be sufficient for the base fees _and_ the full amount in the `gas` field. |
| contractID | [ContractID](#proto-ContractID) |  | The ID of a smart contract to call. |
| gas | [int64](#int64) |  | The amount of &#34;gas&#34; to use for this call. &lt;p&gt; This transaction SHALL consume all of the gas offered and charge the corresponding fee according to the current exchange rate between HBAR and &#34;gas&#34;. |
| functionParameters | [bytes](#bytes) |  | The smart contract function to call, and the parameters to pass to that function. &lt;p&gt; These SHALL be presented in EVM bytecode function call format. |
| maxResultSize | [int64](#int64) |  | **Deprecated.** Do not use this field; it is ignored in the current software. &lt;p&gt; The maximum number of bytes that the result might include.&lt;br/&gt; The call will fail if it would have returned more than this number of bytes. |
| sender_id | [AccountID](#proto-AccountID) |  | The account that is the &#34;sender&#34; for this contract call. &lt;p&gt; If this is not set it SHALL be interpreted as the accountId from the associated transactionId.&lt;br/&gt; If this is set then either the associated transaction or the foreign transaction data MUST be signed by the referenced account. |






<a name="proto-ContractCallLocalResponse"></a>

### ContractCallLocalResponse
The response returned by a `ContractCallLocalQuery` transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| functionResult | [ContractFunctionResult](#proto-ContractFunctionResult) |  | The result(s) returned by the function call, if successful. &lt;p&gt; If the call failed this value SHALL be unset. |





 

 

 

 



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

