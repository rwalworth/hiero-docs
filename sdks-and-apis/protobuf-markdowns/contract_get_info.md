# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/contract_get_info.proto](#services_contract_get_info-proto)
    - [ContractGetInfoQuery](#proto-ContractGetInfoQuery)
    - [ContractGetInfoResponse](#proto-ContractGetInfoResponse)
    - [ContractGetInfoResponse.ContractInfo](#proto-ContractGetInfoResponse-ContractInfo)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_contract_get_info-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/contract_get_info.proto
# Contract Get Info
A standard query to obtain detailed information about a smart contract.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ContractGetInfoQuery"></a>

### ContractGetInfoQuery
Request detailed information about a smart contract.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| contractID | [ContractID](#proto-ContractID) |  | A smart contract ID. &lt;p&gt; The network SHALL return information for this smart contract, if successful. |






<a name="proto-ContractGetInfoResponse"></a>

### ContractGetInfoResponse
Information returned in response to a &#34;get info&#34; query for a smart contract.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| contractInfo | [ContractGetInfoResponse.ContractInfo](#proto-ContractGetInfoResponse-ContractInfo) |  | The information, as requested, for a smart contract. A state proof MAY be generated for this value. |






<a name="proto-ContractGetInfoResponse-ContractInfo"></a>

### ContractGetInfoResponse.ContractInfo



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractID | [ContractID](#proto-ContractID) |  | The ID of the smart contract requested in the query. |
| accountID | [AccountID](#proto-AccountID) |  | The Account ID for the account entry associated with this smart contract. |
| contractAccountID | [string](#string) |  | The &#34;Solidity&#34; form contract ID.&lt;br/&gt; This is a hexadecimal string form of the 20-byte EVM address of the contract. |
| adminKey | [Key](#proto-Key) |  | The key that MUST sign any transaction to update or modify this smart contract. &lt;p&gt; If this value is null, or is an empty `KeyList` then the contract CANNOT be deleted, modified, or updated, but MAY still expire. |
| expirationTime | [Timestamp](#proto-Timestamp) |  | The point in time at which this contract will expire. |
| autoRenewPeriod | [Duration](#proto-Duration) |  | The duration, in seconds, for which the contract lifetime will be automatically extended upon expiration, provide sufficient HBAR is available at that time to pay the renewal fee.&lt;br/&gt; See `auto_renew_account_id` for additional conditions. |
| storage | [int64](#int64) |  | The amount of storage used by this smart contract. |
| memo | [string](#string) |  | A short description of this smart contract. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| balance | [uint64](#uint64) |  | The current HBAR balance, in tinybar, of the smart contract account. |
| deleted | [bool](#bool) |  | A flag indicating that this contract is deleted. |
| tokenRelationships | [TokenRelationship](#proto-TokenRelationship) | repeated | **Deprecated.** Because &lt;a href=&#34;https://hips.hedera.com/hip/hip-367&#34;&gt;HIP-367&lt;/a&gt;, which allows an account to be associated to an unlimited number of tokens, it became necessary to only provide this information from a Mirror Node.&lt;br/&gt; The list of tokens associated to this contract. |
| ledger_id | [bytes](#bytes) |  | The ledger ID of the network that generated this response. &lt;p&gt; This value SHALL identify the distributed ledger that responded to this query. |
| auto_renew_account_id | [AccountID](#proto-AccountID) |  | An account designated to pay the renewal fee upon automatic renewal of this contract. &lt;p&gt; If this is not set, or is set to an account with zero HBAR available, the HBAR balance of the contract, if available, SHALL be used to pay the renewal fee. |
| max_automatic_token_associations | [int32](#int32) |  | The maximum number of tokens that the contract can be associated to automatically. |
| staking_info | [StakingInfo](#proto-StakingInfo) |  | Staking information for this contract. |





 

 

 

 



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

