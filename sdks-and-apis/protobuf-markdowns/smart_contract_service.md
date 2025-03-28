# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/output/smart_contract_service.proto](#block_stream_output_smart_contract_service-proto)
    - [CallContractOutput](#com-hedera-hapi-block-stream-output-CallContractOutput)
    - [CreateContractOutput](#com-hedera-hapi-block-stream-output-CreateContractOutput)
    - [DeleteContractOutput](#com-hedera-hapi-block-stream-output-DeleteContractOutput)
    - [EthereumOutput](#com-hedera-hapi-block-stream-output-EthereumOutput)
    - [SystemDeleteContractOutput](#com-hedera-hapi-block-stream-output-SystemDeleteContractOutput)
    - [SystemUnDeleteContractOutput](#com-hedera-hapi-block-stream-output-SystemUnDeleteContractOutput)
    - [UpdateContractOutput](#com-hedera-hapi-block-stream-output-UpdateContractOutput)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_output_smart_contract_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/output/smart_contract_service.proto
#  Service
Block stream messages that report the results of transactions handled
by the `smart contract` service.

&gt; REVIEW NOTE
&gt;&gt; The use of sidecar records is a bit odd here. We may find it more
&gt;&gt; effective to extract the actual changes into proper output messages
&gt;&gt; and fields included in the ethereum call output and/or related state
&gt;&gt; changes items, and remove the whole sidecar concept going forward.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-output-CallContractOutput"></a>

### CallContractOutput
Block Stream data for a `contractCallMethod` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| sidecars | [proto.TransactionSidecarRecord](#proto-TransactionSidecarRecord) | repeated | A list of additional outputs. &lt;p&gt; This field MAY record one or more additional outputs and smart contract state changes produced during the ethereum call transaction handling.&lt;br/&gt; This field SHALL NOT be set if the transaction handling did not produce additional outputs.&lt;br/&gt; This field is not settled and MAY be removed or modified. |
| contract_call_result | [proto.ContractFunctionResult](#proto-ContractFunctionResult) |  | An EVM contract call result. &lt;p&gt; This field SHALL contain all of the data produced by the contract call transaction as well as basic accounting results. |






<a name="com-hedera-hapi-block-stream-output-CreateContractOutput"></a>

### CreateContractOutput
Block Stream data for a `createContract` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| sidecars | [proto.TransactionSidecarRecord](#proto-TransactionSidecarRecord) | repeated | A list of additional outputs. &lt;p&gt; This field MAY record one or more additional outputs and smart contract state changes produced during the ethereum call transaction handling.&lt;br/&gt; This field SHALL NOT be set if the transaction handling did not produce additional outputs.&lt;br/&gt; This field is not settled and MAY be removed or modified. |
| contract_create_result | [proto.ContractFunctionResult](#proto-ContractFunctionResult) |  | An EVM contract call result. &lt;p&gt; This field SHALL contain all of the data produced by the contract create transaction as well as basic accounting results. |






<a name="com-hedera-hapi-block-stream-output-DeleteContractOutput"></a>

### DeleteContractOutput
Block Stream data for a `deleteContract` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-EthereumOutput"></a>

### EthereumOutput
Block Stream data for a `callEthereum` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| sidecars | [proto.TransactionSidecarRecord](#proto-TransactionSidecarRecord) | repeated | A list of additional outputs. &lt;p&gt; This field MAY record one or more additional outputs and smart contract state changes produced during the ethereum call transaction handling.&lt;br/&gt; This field SHALL NOT be set if the transaction handling did not produce additional outputs.&lt;br/&gt; This field is not settled and MAY be removed or modified. |
| ethereum_hash | [bytes](#bytes) |  | An ethereum hash value. &lt;p&gt; This SHALL be a keccak256 hash of the ethereumData. |
| ethereum_call_result | [proto.ContractFunctionResult](#proto-ContractFunctionResult) |  | A result for an Ethereum Transaction executed as a call. &lt;p&gt; This field SHALL contain all of the data produced by the contract call transaction as well as basic accounting results. |
| ethereum_create_result | [proto.ContractFunctionResult](#proto-ContractFunctionResult) |  | A result for an Ethereum Transaction executed as a create. &lt;p&gt; This field SHALL contain all of the data produced by the contract create transaction as well as basic accounting results. |






<a name="com-hedera-hapi-block-stream-output-SystemDeleteContractOutput"></a>

### SystemDeleteContractOutput
Block Stream data for a contract `systemDelete` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-SystemUnDeleteContractOutput"></a>

### SystemUnDeleteContractOutput
Block Stream data for a contract `systemUndelete` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-UpdateContractOutput"></a>

### UpdateContractOutput
Block Stream data for a `updateContract` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.





 

 

 

 



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

