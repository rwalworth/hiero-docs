# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/contract_types.proto](#services_contract_types-proto)
    - [ContractFunctionResult](#proto-ContractFunctionResult)
    - [ContractLoginfo](#proto-ContractLoginfo)
    - [ContractNonceInfo](#proto-ContractNonceInfo)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_contract_types-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/contract_types.proto
# Contract Message Types
Message types used in contract transactions.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ContractFunctionResult"></a>

### ContractFunctionResult
A contract function result.&lt;br/&gt;
The result returned by a call to a smart contract function. This is part of
the response to a ContractCallLocal query, and is in the record for a
ContractCall. The ContractCreateInstance transaction record also carries a
function result, which is the results of the call to the constructor.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractID | [ContractID](#proto-ContractID) |  | A contract identifier.&lt;br/&gt; This identifies the smart contract that defines the function called. |
| contractCallResult | [bytes](#bytes) |  | Result data from the function call. &lt;p&gt; This SHALL be encoded in RLP bytecode format. |
| errorMessage | [string](#string) |  | Any error message produced by the contract call. &lt;p&gt; This SHALL be unset if the contract call succeeded. |
| bloom | [bytes](#bytes) |  | A bloom filter produced by this contract call.&lt;br/&gt; Ethereum uses this bloom filter to search for call results in the Ethereum block history. High false positive rates make the bloom filters quite limited value. |
| gasUsed | [uint64](#uint64) |  | A quantity of &#34;gas&#34; used.&lt;br/&gt; This represents the resource units expended to execute this contract call, and correlates to transaction costs. |
| logInfo | [ContractLoginfo](#proto-ContractLoginfo) | repeated | Any Log events produced by this contract call. |
| createdContractIDs | [ContractID](#proto-ContractID) | repeated | **Deprecated.** Replaced by values in transaction records to support `CREATE2` calls.&lt;br/&gt; &lt;p&gt; The list of smart contracts that were created by the function call.&lt;br/&gt; The created ids will now _also_ be externalized through internal transaction records, where each record has its alias field populated with the new contract&#39;s EVM address.&lt;br/&gt; This is needed for contracts created with CREATE2, which removes the trivial relationship between a new contract&#39;s Identifier and its Solidity address. |
| evm_address | [google.protobuf.BytesValue](#google-protobuf-BytesValue) |  | A created contract address.&lt;br/&gt; If the function created a new contract (e.g. `CREATE2`), this is the primary 20-byte EVM address for that contract. &lt;p&gt; Every contract SHALL have a &#34;base&#34; EVM address that is determined by its `shard.realm.num` contract ID.&lt;br/&gt; This address is constructed as follows &lt;ol&gt; &lt;li&gt;The first 4 bytes are the big-endian representation of the shard.&lt;/li&gt; &lt;li&gt;The next 8 bytes are the big-endian representation of the realm.&lt;/li&gt; &lt;li&gt;The final 8 bytes are the big-endian representation of the number.&lt;/li&gt; &lt;/ol&gt; &lt;p&gt; Contracts created via `CREATE2` SHALL have an _additional_, primary, address that is derived from the &lt;a href=&#34;https://eips.ethereum.org/EIPS/eip-1014&#34;&gt;&lt;tt&gt;EIP-1014&lt;/tt&gt;&lt;/a&gt; specification. This additional address SHALL NOT be directly related to the `shard.realm.num` contract ID.&lt;br/&gt; It should be emphasized that Contracts created via a `CREATE2` call can also be referenced via the same &#34;base&#34; EVM address as described above. |
| gas | [int64](#int64) |  | The amount of gas available for this call, sometimes referred to as the gasLimit.&lt;br/&gt; This field SHALL NOT be populated when the associated `TransactionBody` in the block stream is a `ContractCreateTransactionBody` or a `ContractCallTransactionBody`. |
| amount | [int64](#int64) |  | An amount, in tinybar, sent by this function call.&lt;br/&gt; This SHALL be zero(0) if the function called is not `payable`.&lt;br/&gt; This field SHALL NOT be populated when the associated `TransactionBody` in the block stream is a `ContractCreateTransactionBody` or a `ContractCallTransactionBody`. |
| functionParameters | [bytes](#bytes) |  | The smart contract function to call, and the parameters to pass to that function.&lt;br/&gt; These SHALL be presented in EVM bytecode function call format.&lt;br/&gt; This field SHALL NOT be populated when the associated `TransactionBody` in the block stream is a `ContractCreateTransactionBody` or a `ContractCallTransactionBody`. |
| sender_id | [AccountID](#proto-AccountID) |  | The account that was the &#34;sender&#34; for this contract call.&lt;br/&gt; If this is not set it SHALL be read from the accountId in the transactionId for the contract call.&lt;br/&gt; This field SHALL NOT be populated when the associated `TransactionBody` in the block stream is a `ContractCreateTransactionBody` or a `ContractCallTransactionBody`. |
| contract_nonces | [ContractNonceInfo](#proto-ContractNonceInfo) | repeated | A list of contract account nonce values.&lt;br/&gt; This list SHALL contain a nonce value for each contract account modified as a result of this contract call. These nonce values SHALL be the value after the contract call is completed. |
| signer_nonce | [google.protobuf.Int64Value](#google-protobuf-Int64Value) |  | A nonce value for the &#34;signer account&#34;.&lt;br/&gt; If the contract call updated the signer nonce for the signer account (i.e. by creating another contract), this field SHALL contain the updated value.&lt;br/&gt; If the signer account nonce was not updated, this field SHALL be `null`. |






<a name="proto-ContractLoginfo"></a>

### ContractLoginfo
EVM log data for a contract call.&lt;br/&gt;
The EVM log information produced by a smart contract function call.

Each contract function call MAY return zero or more log events.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractID | [ContractID](#proto-ContractID) |  | A contract identifier.&lt;br/&gt; This refers to the contract that generated this log entry. |
| bloom | [bytes](#bytes) |  | A bloom filter.&lt;br/&gt; This filter applies to this log entry and indexes the contract log data in the full data of the Ethereum block.&lt;br/&gt; EIP-7668 proposes to remove bloom filters as they are quite low value in practice and separate indexing services are more effective. |
| topic | [bytes](#bytes) | repeated | A list of the &#34;topics&#34; in this log entry.&lt;br/&gt; The EVM permits up to 4 topics, each of which is 32 bytes (one EVM word). &lt;p&gt; The first &#34;topic&#34; is special, and MUST contain the keccak256 hash of the event signature, if the event is not anonymous. |
| data | [bytes](#bytes) |  | Event data for this log entry.&lt;br/&gt; This is binary data consisting of an arbitrary number of 256 bit (32 byte) words. The content of that data is determined by the smart contract code.&lt;br/&gt; |






<a name="proto-ContractNonceInfo"></a>

### ContractNonceInfo
A contract &#34;nonce&#34; reference.&lt;br/&gt;
This connects a contract and its &#34;nonce&#34; value, and is primarily for use in
query responses.  A &#34;nonce&#34; is short for &#34;nonsense&#34; and is usually a value
with no particular meaning.

The nonce of a contract SHALL be incremented when that contract creates
another contract.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contract_id | [ContractID](#proto-ContractID) |  | A contract identifier.&lt;br/&gt; This refers to the contract that holds this nonce value. |
| nonce | [int64](#int64) |  | A &#34;nonce&#34; value. The current value of the nonce associated with the identified contract. |





 

 

 

 



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

