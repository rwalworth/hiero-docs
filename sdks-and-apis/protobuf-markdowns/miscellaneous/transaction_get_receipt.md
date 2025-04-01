# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/transaction_get_receipt.proto](#services_transaction_get_receipt-proto)
    - [TransactionGetReceiptQuery](#proto-TransactionGetReceiptQuery)
    - [TransactionGetReceiptResponse](#proto-TransactionGetReceiptResponse)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_transaction_get_receipt-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/transaction_get_receipt.proto
# Get Transaction Receipt
This query is central to client interactions. A client must query
the network for the &#34;receipt&#34; after a transaction is submitted to know
whether the transaction succeeded and the consensus result.

&gt; Implementation Note
&gt;&gt; This query is _defined_ for &#34;Crypto&#34; service, but is _implemented_ by
&gt;&gt; the &#34;Network Admin&#34; service.

&gt; Note
&gt;&gt; The mechanism for transaction receipts and results is subject to
&gt;&gt; considerable change in the near future. Clients heavily dependent
&gt;&gt; on direct network queries for transaction receipts may consider
&gt;&gt; changes needed to query a mirror node for transaction receipts
&gt;&gt; and results instead.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TransactionGetReceiptQuery"></a>

### TransactionGetReceiptQuery
A query to retrieve a transaction receipt.
This query retrieves the post-consensus (final) result of a transaction.
A transaction receipt may not be available if queried too early
(less than 5-10 seconds), or too late (more than 3 minutes). If a receipt
is available, it contains basic transaction results. A query to a mirror
node (or other archival system) is required to obtain full detail for a
transaction, or any result after the basic receipt time period.

This query is &#34;free&#34;. The payment field in the header MUST be empty.&lt;br/&gt;
If a receipt is not available, the response SHALL be `UNKNOWN`.&lt;br/&gt;
A transaction receipt SHALL be available after the network reaches
consensus for a transaction.&lt;br/&gt;
A transaction receipt SHALL NOT be available after the end of the network
configured &#34;receipt period&#34;, typically three(3) minutes.

&lt;dl&gt;
  &lt;dt&gt;What is the &#34;first&#34; transaction?&lt;/dt&gt;
  &lt;dd&gt;The &#34;first&#34; transaction SHALL be the the transaction with
      the earliest consensus time and a status that is neither
      `INVALID_NODE_ACCOUNT` nor `INVALID_PAYER_SIGNATURE`.&lt;br/&gt;
      If no transaction is found meeting this status criteria, the
      &#34;first&#34; transaction SHALL be the transaction with the earliest
      consensus time.&lt;/dd&gt;
 &lt;dt&gt;What is a &#34;child&#34; transaction?&lt;/dt&gt;
 &lt;dd&gt;A &#34;child&#34; transaction is any transaction created in the process of
     completing another transaction. These are most common with a smart
     contract call, where a call to a contract may initiate one or more
     additional transactions to complete a complex process.&lt;/dd&gt;
&lt;/dl&gt;


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| transactionID | [TransactionID](#proto-TransactionID) |  | A transaction identifier. &lt;p&gt; This MUST contain the full identifier, as submitted, for the transaction to query. |
| includeDuplicates | [bool](#bool) |  | A flag to request duplicates. &lt;p&gt; If set, every transaction receipt within the receipt period that matches the requested transaction identifier SHALL be returned.&lt;br/&gt; If not set, duplicate transactions SHALL NOT be returned.&lt;br/&gt; If not set, only the receipt for the first matching transaction to reach consensus SHALL be returned. |
| include_child_receipts | [bool](#bool) |  | A flag to request &#34;child&#34; receipts. &lt;p&gt; If set, the response SHALL include receipts for each child transaction executed as part of the requested parent transaction.&lt;br/&gt; If not set, the response SHALL NOT include any receipts for child transactions. |






<a name="proto-TransactionGetReceiptResponse"></a>

### TransactionGetReceiptResponse
Response message for a `getTransactionReceipts` query.

The `receipt` field SHALL return the receipt for the &#34;first&#34; transaction
that matches the transaction identifier requested.&lt;br/&gt;
If receipts for duplicate transactions are requested, those duplicate
receipts SHALL be present in the `duplicateTransactionReceipts` list.&lt;br/&gt;
If receipts for child transactions are requested, those child receipts
SHALL be present in the `child_transaction_receipts` list.&lt;br/&gt;
A state proof SHALL NOT be provided for this response; transaction receipts
are not retained in network state.

&lt;dl&gt;
  &lt;dt&gt;What is the &#34;first&#34; transaction?&lt;/dt&gt;
  &lt;dd&gt;The &#34;first&#34; transaction receipt SHALL be the receipt for the
      first transaction with status that is neither
      `INVALID_NODE_ACCOUNT` nor `INVALID_PAYER_SIGNATURE`.&lt;br/&gt;
      If no transaction is found meeting the status criteria, the
      &#34;first&#34; transaction SHALL be the first transaction by
      consensus time.&lt;/dd&gt;
 &lt;dt&gt;What is a &#34;child&#34; transaction?&lt;/dt&gt;
 &lt;dd&gt;A &#34;child&#34; transaction is any transaction created in the process of
     completing another transaction. These are most common with a smart
     contract call, where a call to a contract may initiate one or more
     additional transactions to complete a complex process.&lt;/dd&gt;
&lt;/dl&gt;


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| receipt | [TransactionReceipt](#proto-TransactionReceipt) |  | A transaction receipt. &lt;p&gt; This SHALL be the receipt for the &#34;first&#34; transaction that matches the transaction identifier requested.&lt;br/&gt; If the identified transaction has not reached consensus, this receipt SHALL have a `status` of `UNKNOWN`.&lt;br/&gt; If the identified transaction reached consensus prior to the current configured receipt period (typically the last 180 seconds), this receipt SHALL have a `status` of `UNKNOWN`. |
| duplicateTransactionReceipts | [TransactionReceipt](#proto-TransactionReceipt) | repeated | A list of duplicate transaction receipts. &lt;p&gt; If the request set the `includeDuplicates` flat, this list SHALL include the receipts for each duplicate transaction associated to the requested transaction identifier. If the request did not set the `includeDuplicates` flag, this list SHALL be empty.&lt;br/&gt; If the `receipt` status is `UNKNOWN`, this list SHALL be empty.&lt;br/&gt; This list SHALL be in order by consensus timestamp. |
| child_transaction_receipts | [TransactionReceipt](#proto-TransactionReceipt) | repeated | A list of receipts for all child transactions spawned by the requested transaction. &lt;p&gt; If the request set the `include_child_receipts` flag, this list SHALL include receipts for each child transaction executed as part of the requested parent transaction.&lt;br/&gt; If the request did not set the `include_child_receipts` flag, this list SHALL be empty. &lt;br/&gt; If the parent transaction did not initiate any child transactions this list SHALL be empty.&lt;br/&gt; If the `receipt` status is `UNKNOWN`, this list SHALL be empty.&lt;br/&gt; This list SHALL be in order by consensus timestamp. |





 

 

 

 



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

