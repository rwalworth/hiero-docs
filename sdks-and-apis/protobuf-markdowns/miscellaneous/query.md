# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/query.proto](#services_query-proto)
    - [Query](#proto-Query)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_query-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/query.proto
# Query
This is the parent message for all queries, and this message is
serialized and signed, with the signature included in the QueryHeader.

All of the entries in the `query` `oneof` are fully specified elsewhere;
we only include a short summary here.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-Query"></a>

### Query
A query transaction.&lt;br/&gt;
This message is serialized to bytes and those bytes are signed by the
submitter, with the signature included in the QueryHeader for the query
request.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| getByKey | [GetByKeyQuery](#proto-GetByKeyQuery) |  | Get all entities associated with a given key. |
| getBySolidityID | [GetBySolidityIDQuery](#proto-GetBySolidityIDQuery) |  | Get an Hedera identifier associated with an identifier in EVM &#34;Solidity&#34; form.&lt;br/&gt; Most often used in smart contracts to find an Hedera account, file, or contract identifier to pass to a system contract or precompile. |
| contractCallLocal | [ContractCallLocalQuery](#proto-ContractCallLocalQuery) |  | Call a function of a smart contract.&lt;br/&gt; This call is executed exclusively on the node to which it is submitted, and is much less expensive than a `contractCall` transaction. |
| contractGetInfo | [ContractGetInfoQuery](#proto-ContractGetInfoQuery) |  | Get information about a smart contract. |
| contractGetBytecode | [ContractGetBytecodeQuery](#proto-ContractGetBytecodeQuery) |  | Get runtime bytecode used by a smart contract. |
| ContractGetRecords | [ContractGetRecordsQuery](#proto-ContractGetRecordsQuery) |  | **Deprecated.** This query is unsupported and SHALL fail.&lt;br/&gt; Requests for this information MUST be directed to a mirror node. &lt;p&gt; Get Records of a smart contract. |
| cryptogetAccountBalance | [CryptoGetAccountBalanceQuery](#proto-CryptoGetAccountBalanceQuery) |  | Get the current HBAR balance of an Hedera account or smart contract. |
| cryptoGetAccountRecords | [CryptoGetAccountRecordsQuery](#proto-CryptoGetAccountRecordsQuery) |  | Get records of all &#34;recent&#34; transactions for which a specified account is the effective payer. |
| cryptoGetInfo | [CryptoGetInfoQuery](#proto-CryptoGetInfoQuery) |  | Get information about an account, including the balance.&lt;br/&gt; This does not get the list of account records. |
| cryptoGetLiveHash | [CryptoGetLiveHashQuery](#proto-CryptoGetLiveHashQuery) |  | **Deprecated.** This query is unsupported and SHALL fail.&lt;br/&gt; Requests for this information MUST be directed to a mirror node. &lt;p&gt; Get a single livehash from a single account, if present. |
| cryptoGetProxyStakers | [CryptoGetStakersQuery](#proto-CryptoGetStakersQuery) |  | **Deprecated.** This query is unsupported and SHALL fail.&lt;br/&gt; Requests for this information MUST be directed to a mirror node. &lt;p&gt; Get all the accounts that are proxy staking to this account. |
| fileGetContents | [FileGetContentsQuery](#proto-FileGetContentsQuery) |  | Get the content of a file. |
| fileGetInfo | [FileGetInfoQuery](#proto-FileGetInfoQuery) |  | Get metadata for a file. |
| transactionGetReceipt | [TransactionGetReceiptQuery](#proto-TransactionGetReceiptQuery) |  | Get a receipt for a transaction.&lt;br/&gt; This only returns a receipt if the transaction is &#34;recent&#34;, which is typically within the previous 180 seconds (3 minutes). |
| transactionGetRecord | [TransactionGetRecordQuery](#proto-TransactionGetRecordQuery) |  | Get a record for a transaction. This only returns a record if the transaction is &#34;available&#34;, which is typically within the previous 1 hour. |
| transactionGetFastRecord | [TransactionGetFastRecordQuery](#proto-TransactionGetFastRecordQuery) |  | Get a record for a transaction. This only returns a record if the transaction is &#34;recent&#34;, which is typically within the previous 180 seconds (3 minutes). |
| consensusGetTopicInfo | [ConsensusGetTopicInfoQuery](#proto-ConsensusGetTopicInfoQuery) |  | Get metadata for a consensus topic. |
| networkGetVersionInfo | [NetworkGetVersionInfoQuery](#proto-NetworkGetVersionInfoQuery) |  | Get the versions of Hedera Services and the HAPI API deployed on the responding consensus node. |
| tokenGetInfo | [TokenGetInfoQuery](#proto-TokenGetInfoQuery) |  | Get metadata for a token. |
| scheduleGetInfo | [ScheduleGetInfoQuery](#proto-ScheduleGetInfoQuery) |  | Get metadata for a schedule.&lt;br/&gt; A schedule is a request to execute a transaction at a future time. |
| tokenGetAccountNftInfos | [TokenGetAccountNftInfosQuery](#proto-TokenGetAccountNftInfosQuery) |  | **Deprecated.** This query is unsupported and SHALL fail.&lt;br/&gt; Requests for this information MUST be directed to a mirror node. &lt;p&gt; Get a list of non-fungible/unique tokens associated with an account. |
| tokenGetNftInfo | [TokenGetNftInfoQuery](#proto-TokenGetNftInfoQuery) |  | Get metadata for a specific, serial numbered, non-fungible/unique token (NFT). |
| tokenGetNftInfos | [TokenGetNftInfosQuery](#proto-TokenGetNftInfosQuery) |  | **Deprecated.** This query is unsupported and SHALL fail.&lt;br/&gt; Requests for this information MUST be directed to a mirror node. &lt;p&gt; Get metadata for all non-fungible/unique tokens (NFTs) of a single type within a range of indices (0-based count of minted tokens). |
| networkGetExecutionTime | [NetworkGetExecutionTimeQuery](#proto-NetworkGetExecutionTimeQuery) |  | **Deprecated.** This query is unsupported and SHALL fail.&lt;br/&gt; Requests for this information MUST be directed to a mirror node. &lt;p&gt; Get the execution time for a recent transaction. |
| accountDetails | [GetAccountDetailsQuery](#proto-GetAccountDetailsQuery) |  | Get detail metadata for an account. |





 

 

 

 



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

