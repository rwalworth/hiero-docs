# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/transaction_record.proto](#services_transaction_record-proto)
    - [PendingAirdropRecord](#proto-PendingAirdropRecord)
    - [TransactionRecord](#proto-TransactionRecord)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_transaction_record-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/transaction_record.proto
# Transaction Record
The record of a single transaction, including receipt and transaction
results such as transfer lists, entropy, contract call result, etc...&lt;br/&gt;
The record also includes fees, consensus time, EVM information, and
other result metadata.&lt;br/&gt;
Only values appropriate to the requested transaction are populated, all
other fields will not be set (i.e. null or default values).

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-PendingAirdropRecord"></a>

### PendingAirdropRecord
A record of a new pending airdrop.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| pending_airdrop_id | [PendingAirdropId](#proto-PendingAirdropId) |  | A unique, composite, identifier for a pending airdrop. &lt;p&gt; This field is REQUIRED. |
| pending_airdrop_value | [PendingAirdropValue](#proto-PendingAirdropValue) |  | A single pending airdrop amount. &lt;p&gt; If the pending airdrop is for a fungible/common token this field is REQUIRED and SHALL be the current amount of tokens offered.&lt;br/&gt; If the pending airdrop is for a non-fungible/unique token, this field SHALL NOT be set. |






<a name="proto-TransactionRecord"></a>

### TransactionRecord
Response when the client sends the node TransactionGetRecordResponse


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| receipt | [TransactionReceipt](#proto-TransactionReceipt) |  | A transaction receipt. &lt;p&gt; This SHALL report consensus status (reach consensus, failed, unknown) and the ID of any new entity (i.e. account, file, contract, schedule, etc...) created. |
| transactionHash | [bytes](#bytes) |  | A transaction hash value. &lt;p&gt; This SHALL be the hash of the Transaction that executed and SHALL NOT be the hash of any Transaction that failed for having a duplicate TransactionID. |
| consensusTimestamp | [Timestamp](#proto-Timestamp) |  | A consensus timestamp. &lt;p&gt; This SHALL be null if the transaction did not reach consensus yet. |
| transactionID | [TransactionID](#proto-TransactionID) |  | A transaction identifier to the transaction associated to this record. |
| memo | [string](#string) |  | A transaction memo.&lt;br/&gt; This is the memo that was submitted as part of the transaction. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| transactionFee | [uint64](#uint64) |  | A transaction fee charged. &lt;p&gt; This SHALL be the actual transaction fee charged.&lt;br/&gt; This MAY NOT match the original `transactionFee` value from the `TransactionBody`. |
| contractCallResult | [ContractFunctionResult](#proto-ContractFunctionResult) |  | A contract call result.&lt;br/&gt; A record of the value returned by the smart contract function (if it completed and didn&#39;t fail) from a `ContractCallTransaction`. |
| contractCreateResult | [ContractFunctionResult](#proto-ContractFunctionResult) |  | A contract creation result.&lt;br/&gt; A record of the value returned by the smart contract constructor (if it completed and didn&#39;t fail) from a `ContractCreateTransaction`. |
| transferList | [TransferList](#proto-TransferList) |  | A transfer list for this transaction.&lt;br/&gt; This is a list of all HBAR transfers completed for this transaction. &lt;p&gt; This MAY include fees, transfers performed by the transaction, transfers initiated by a smart contract it calls, or the creation of threshold records that it triggers. |
| tokenTransferLists | [TokenTransferList](#proto-TokenTransferList) | repeated | A token transfer list for this transaction.&lt;br/&gt; This is a list of all non-HBAR token transfers completed for this transaction.&lt;br/&gt; |
| scheduleRef | [ScheduleID](#proto-ScheduleID) |  | A schedule reference.&lt;br/&gt; The reference to a schedule ID for the schedule that initiated this transaction, if this this transaction record represents a scheduled transaction. |
| assessed_custom_fees | [AssessedCustomFee](#proto-AssessedCustomFee) | repeated | A list of all custom fees that were assessed during a CryptoTransfer. &lt;p&gt; These SHALL be paid if the transaction status resolved to SUCCESS. |
| automatic_token_associations | [TokenAssociation](#proto-TokenAssociation) | repeated | A list of all token associations implicitly or automatically created while handling this transaction. |
| parent_consensus_timestamp | [Timestamp](#proto-Timestamp) |  | A consensus timestamp for a child record. &lt;p&gt; This SHALL be the consensus timestamp of a user transaction that spawned an internal child transaction. |
| alias | [bytes](#bytes) |  | A new account alias.&lt;br/&gt; &lt;p&gt; This is the new alias assigned to an account created as part of a CryptoCreate transaction triggered by a user transaction with a (previously unused) alias. |
| ethereum_hash | [bytes](#bytes) |  | A keccak256 hash of the ethereumData. &lt;p&gt; This field SHALL only be populated for EthereumTransaction. |
| paid_staking_rewards | [AccountAmount](#proto-AccountAmount) | repeated | A list of staking rewards paid. &lt;p&gt; This SHALL be a list accounts with the corresponding staking rewards paid as a result of this transaction. |
| prng_bytes | [bytes](#bytes) |  | A pseudorandom 384-bit sequence. &lt;p&gt; This SHALL be returned in the record of a UtilPrng transaction with no output range, |
| prng_number | [int32](#int32) |  | A pseudorandom 32-bit integer.&lt;br/&gt; &lt;p&gt; This SHALL be returned in the record of a PRNG transaction with an output range specified. |
| evm_address | [bytes](#bytes) |  | A new default EVM address for an account created by this transaction. &lt;p&gt; This field SHALL be populated only when the EVM address is not specified in the related transaction body. |
| new_pending_airdrops | [PendingAirdropRecord](#proto-PendingAirdropRecord) | repeated | A list of pending token airdrops. &lt;p&gt; Each pending airdrop SHALL represent a single requested transfer from a sending account to a recipient account.&lt;br/&gt; These pending transfers are issued unilaterally by the sending account, and MUST be claimed by the recipient account before the transfer SHALL complete.&lt;br/&gt; A sender MAY cancel a pending airdrop before it is claimed.&lt;br/&gt; An airdrop transaction SHALL emit a pending airdrop when the recipient has no available automatic association slots available or when the recipient has set `receiver_sig_required`. |





 

 

 

 



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

