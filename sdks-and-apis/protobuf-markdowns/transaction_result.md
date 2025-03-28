# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/output/transaction_result.proto](#block_stream_output_transaction_result-proto)
    - [TransactionResult](#com-hedera-hapi-block-stream-output-TransactionResult)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_output_transaction_result-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/output/transaction_result.proto
# Transaction Result
The result of a transaction, sometimes called a receipt.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-output-TransactionResult"></a>

### TransactionResult
While we have the state changes as part of the block stream,
we may not have the full data set needed. To surface information
such as staking rewards, fees, etc. we need to include some of the
data from the original TransactionRecord.

&gt; REVIEW NOTE
&gt;&gt; Should we have custom fees here, and remove that from the
&gt;&gt; CryptoTransfer output message? That would make more sense, as I believe
&gt;&gt; TokenTransfer output would also need custom fees, and we may wish
&gt;&gt; to add custom fees to other transactions in the future.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| status | [proto.ResponseCodeEnum](#proto-ResponseCodeEnum) |  | A response code. &lt;p&gt; This value SHALL indicate the status of this transaction.&lt;br/&gt; This code SHALL indicate success or a specific failure. |
| consensus_timestamp | [proto.Timestamp](#proto-Timestamp) |  | A consensus timestamp. &lt;p&gt; The time index, agreed by all network nodes, when this transaction reached consensus.&lt;br/&gt; This field SHALL be set for all transactions. |
| parent_consensus_timestamp | [proto.Timestamp](#proto-Timestamp) |  | A consensus timestamp. &lt;p&gt; The time index, agreed by all network nodes, when the &#34;parent&#34; transaction, if any, for this transaction reached consensus.&lt;br/&gt; This SHALL NOT be set on a user-submitted transaction.&lt;br/&gt; This SHALL be set on an internal &#34;child&#34; transaction initiated as part of completing a user-submitted transaction. |
| schedule_ref | [proto.ScheduleID](#proto-ScheduleID) |  | A schedule that executed this transaction, if this transaction was scheduled. &lt;p&gt; This value SHALL NOT be set unless this transaction result represents the result of a _scheduled_ child transaction. |
| transaction_fee_charged | [uint64](#uint64) |  | An amount, in tinybar, charged for this transaction. &lt;p&gt; This SHALL be the actual transaction fee charged, and SHALL NOT be the transactionFee value from TransactionBody. |
| transfer_list | [proto.TransferList](#proto-TransferList) |  | A list of HBAR transfers, in double-entry form. &lt;p&gt; This SHALL include all HBAR transfers completed as a result of this transaction.&lt;br/&gt; This MUST include, at least, &lt;ul&gt; &lt;li&gt;Each source and recipient of transaction fees&lt;/li&gt; &lt;li&gt;All transfers directly performed by this transaction&lt;/li&gt; &lt;li&gt;All transfers due to staking rewards paid as a result of this transaction&lt;/li&gt; &lt;li&gt;Any transfers performed by a smart contract call associated with this transaction&lt;/li&gt; &lt;li&gt;Any transfers caused by the creation of threshold records&lt;/li&gt; &lt;/ul&gt; &lt;p&gt; This transfer list is exposed in the Mirror Node API (MAPI) and in use by clients of the MAPI. Additionally, there are some transfers that are mingled with transactions and only split out here. |
| token_transfer_lists | [proto.TokenTransferList](#proto-TokenTransferList) | repeated | A list of _non-HBAR_ token transfers, in single-entry form. &lt;p&gt; This SHALL include all _non-HBAR_ token transfers completed as a result of this transaction.&lt;br/&gt; This MUST include, at least, &lt;ul&gt; &lt;li&gt;Each source and recipient of custom fees&lt;/li&gt; &lt;li&gt;All transfers directly performed by this transaction&lt;/li&gt; &lt;li&gt;Any transfers performed by a smart contract call associated with this transaction&lt;/li&gt; &lt;/ul&gt; |
| automatic_token_associations | [proto.TokenAssociation](#proto-TokenAssociation) | repeated | A list of token associations. &lt;p&gt; This field SHALL list all token associations created automatically while handling this transaction. |
| paid_staking_rewards | [proto.AccountAmount](#proto-AccountAmount) | repeated | A list of accounts and amounts. &lt;p&gt; This SHALL list all accounts paid staking rewards as a result of this transaction.&lt;br/&gt; Each entry SHALL contain both the account and the amount paid. |
| congestion_pricing_multiplier | [uint64](#uint64) |  | A congestion pricing multiplier. &lt;p&gt; This SHALL be the multiplier that is applied to the transaction fees charged for this transaction. |
| assessed_custom_fees | [proto.AssessedCustomFee](#proto-AssessedCustomFee) | repeated | Custom fees assessed during a transaction execution. &lt;p&gt; These fees SHALL be present in the full transfer list for the transaction. |





 

 

 

 



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

