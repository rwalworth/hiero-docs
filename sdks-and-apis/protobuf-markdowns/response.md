# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/response.proto](#services_response-proto)
    - [Response](#proto-Response)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_response-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/response.proto
# Query Response
The `Response` message is returned from a query transaction.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-Response"></a>

### Response
A single query response.

Each query MUST define its specific response type.&lt;br/&gt;
Each query response MUST include both the information request and a
`ResponseHeader`.&lt;br/&gt;
All possible query response types MUST be listed here in a `oneof`.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| getByKey | [GetByKeyResponse](#proto-GetByKeyResponse) |  | A response for a query requesting all accounts, claims, files, and smart contract instances whose associated keys include the given Key. &lt;p&gt; &gt; This query is no longer supported. |
| getBySolidityID | [GetBySolidityIDResponse](#proto-GetBySolidityIDResponse) |  | A response for a query requesting Contract, Account, and File identifiers for a smart contract, given a Solidity identifier. |
| contractCallLocal | [ContractCallLocalResponse](#proto-ContractCallLocalResponse) |  | A response for a _local_ call to a query function of a given smart contract, providing function parameter inputs as needed. &lt;p&gt; &gt; This call is only performed on the local node. It is _not_ a &gt; network consensus result. |
| contractGetBytecodeResponse | [ContractGetBytecodeResponse](#proto-ContractGetBytecodeResponse) |  | A response for a query requesting the current bytecode for a smart contract. |
| contractGetInfo | [ContractGetInfoResponse](#proto-ContractGetInfoResponse) |  | A response for a query requesting detailed information about a smart contract. |
| contractGetRecordsResponse | [ContractGetRecordsResponse](#proto-ContractGetRecordsResponse) |  | A response for a query requesting records of all transactions against the given contract in the last 25 hours. &lt;p&gt; &gt; This query is no longer supported. |
| cryptogetAccountBalance | [CryptoGetAccountBalanceResponse](#proto-CryptoGetAccountBalanceResponse) |  | A response for a query requesting the HBAR balance of an account or contract. |
| cryptoGetAccountRecords | [CryptoGetAccountRecordsResponse](#proto-CryptoGetAccountRecordsResponse) |  | A response for a query requesting records of all &#34;recent&#34; transactions for which the specified account is the effective payer. |
| cryptoGetInfo | [CryptoGetInfoResponse](#proto-CryptoGetInfoResponse) |  | A response for a query requesting information for an account.&lt;br/&gt; This query includes balance, but not allowances or token relationships. |
| cryptoGetLiveHash | [CryptoGetLiveHashResponse](#proto-CryptoGetLiveHashResponse) |  | A response for a query requesting detail for a specific live hash associated to a specific account. &lt;p&gt; &gt; This query is no longer supported. |
| cryptoGetProxyStakers | [CryptoGetStakersResponse](#proto-CryptoGetStakersResponse) |  | A response for a query requesting all the accounts that are proxy staking to this account. &lt;p&gt; &gt; This query is no longer supported. |
| fileGetContents | [FileGetContentsResponse](#proto-FileGetContentsResponse) |  | A response for a query requesting the content of a file in the Hedera File Service (HFS). |
| fileGetInfo | [FileGetInfoResponse](#proto-FileGetInfoResponse) |  | A response for a query requesting file metadata from the Hedera File Service (HFS). |
| transactionGetReceipt | [TransactionGetReceiptResponse](#proto-TransactionGetReceiptResponse) |  | A response for a query requesting the post-consensus (final) result of a transaction. |
| transactionGetRecord | [TransactionGetRecordResponse](#proto-TransactionGetRecordResponse) |  | A response for a query requesting a transaction record; the detail changes completed in response to a transaction. |
| transactionGetFastRecord | [TransactionGetFastRecordResponse](#proto-TransactionGetFastRecordResponse) |  | A response for a query requesting a very recent transaction record. &lt;p&gt; &gt; This query is no longer supported. |
| consensusGetTopicInfo | [ConsensusGetTopicInfoResponse](#proto-ConsensusGetTopicInfoResponse) |  | A response for a query requesting the current state of a topic for the Hedera Consensus Service (HCS). |
| networkGetVersionInfo | [NetworkGetVersionInfoResponse](#proto-NetworkGetVersionInfoResponse) |  | A response for a query requesting the deployed versions of Hedera Services and the API definitions in semantic version format |
| tokenGetInfo | [TokenGetInfoResponse](#proto-TokenGetInfoResponse) |  | A response for a query requesting metadata for a specific Token. |
| scheduleGetInfo | [ScheduleGetInfoResponse](#proto-ScheduleGetInfoResponse) |  | A response for a query requesting detail for a scheduled transaction. |
| tokenGetAccountNftInfos | [TokenGetAccountNftInfosResponse](#proto-TokenGetAccountNftInfosResponse) |  | A response for a query requesting detail for a subset of individual non-fungible/unique tokens owned by an account.&lt;br/&gt; The requested tokens are selected by a list index, which is based on the order in which the tokens were added to the account. |
| tokenGetNftInfo | [TokenGetNftInfoResponse](#proto-TokenGetNftInfoResponse) |  | A response for a query requesting detail for a specific non-fungible/unique token selected by both token identifier and serial number. |
| tokenGetNftInfos | [TokenGetNftInfosResponse](#proto-TokenGetNftInfosResponse) |  | A response for a query requesting detail for a subset of individual non-fungible/unique tokens.&lt;br/&gt; The requested tokens are selected by a list index, which is based on the order in which the tokens were minted. |
| networkGetExecutionTime | [NetworkGetExecutionTimeResponse](#proto-NetworkGetExecutionTimeResponse) |  | A response for a query requesting the time, in nanoseconds, spent in direct processing for one or more recent transactions. &lt;p&gt; &gt; This query is no longer supported. |
| accountDetails | [GetAccountDetailsResponse](#proto-GetAccountDetailsResponse) |  | A response for a query requesting detail information about an account. &lt;p&gt; This query is a privileged query and &#34;system&#34; account authorization is REQUIRED for this query. |





 

 

 

 



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

