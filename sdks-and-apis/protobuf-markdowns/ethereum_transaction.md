# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/ethereum_transaction.proto](#services_ethereum_transaction-proto)
    - [EthereumTransactionBody](#proto-EthereumTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_ethereum_transaction-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/ethereum_transaction.proto
# Ethereum Call
Make an Ethereum transaction &#34;call&#34; with all data in Ethereum formats,
including the contract alias. Call data may be in the transaction,
or stored within an Hedera File.&lt;br/&gt;
The caller MAY offer additional gas above what is offered in the call
data, but MAY be charged up to 80% of that value if the amount required
is less than this &#34;floor&#34; amount.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-EthereumTransactionBody"></a>

### EthereumTransactionBody
A transaction in Ethereum format.&lt;br/&gt;
Make an Ethereum transaction &#34;call&#34; with all data in Ethereum formats,
including the contract alias. Call data may be in the transaction, or
stored within an Hedera File.

The caller MAY offer additional gas above what is offered in the call data,
but MAY be charged up to 80% of that value if the amount required is less
than this &#34;floor&#34; amount.

### Block Stream Effects
An `EthereumOutput` message SHALL be emitted for each transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ethereum_data | [bytes](#bytes) |  | The raw Ethereum transaction data. &lt;p&gt; This transaction MUST be RLP encoded.&lt;br/&gt; This SHALL be the complete transaction data unless the `call_data` field is set.&lt;br/&gt; If `call_data` is set, this field SHALL be modified to replace the `callData` element with the content of the referenced file.&lt;br/&gt; The transaction signature SHALL be validated after `callData` is complete, if necessary. |
| call_data | [FileID](#proto-FileID) |  | The `callData` for the Ethereum transaction. &lt;p&gt; If this field is set, the data in the `ethereum_data` field SHALL be re-written to replace the `callData` element with the contents of this file at time of execution.&lt;br/&gt; The Ethereum transaction MUST be &#34;rehydrated&#34; with this modified `callData` before signature validation MAY be performed. |
| max_gas_allowance | [int64](#int64) |  | A maximum amount of &#34;gas&#34; offered to pay the Ethereum transaction costs. &lt;p&gt; This gas offered is in addition to any gas supplied with the Ethereum transaction as declared in the `ethereum_data`.&lt;br/&gt; In most circumstances the account with an alias matching the public key available from the Ethereum transaction signature offers sufficient gas to power the transaction, but in some cases it MAY be desirable for the account submitting this transaction to either supplement or entirely fund the transaction cost.&lt;br/&gt; The amount of gas offered here SHALL be used to pay for transaction costs _in excess_ of any gas offered within the Ethereum transaction.&lt;br/&gt; If the gas offered within the Ethereum transaction is sufficient for all costs, the gas offered in this field SHALL NOT be expended.&lt;br/&gt; Regardless of actual transaction cost, the payer for this transaction SHALL NOT be charged more gas than the amount offered here.&lt;br/&gt; If the sum of both gas amounts is not sufficient to pay for the transaction, the entire total amount of gas offered SHALL be expended, the transaction SHALL fail, and the response code `INSUFFICIENT_GAS` SHALL be set.&lt;br/&gt; If any amount of gas is charged to the payer of this transaction, at least 80% of the value offered in this field SHALL be charged as a minimum fee.&lt;br/&gt; If the amount of gas authorized in the Ethereum transaction data is `0`, then the payer of this transaction SHALL be charged the entire cost of the Ethereum transaction, subject to the limit set in this field. |





 

 

 

 



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

