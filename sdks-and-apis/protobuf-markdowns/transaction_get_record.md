# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/transaction_get_record.proto](#services_transaction_get_record-proto)
    - [TransactionGetRecordQuery](#proto-TransactionGetRecordQuery)
    - [TransactionGetRecordResponse](#proto-TransactionGetRecordResponse)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_transaction_get_record-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/transaction_get_record.proto
# Get Transaction Record
Messages for a query to obtain a transaction record. This particular
query is used by `getTxRecordByTxID` in the &#34;Crypto&#34; service API.

&gt; Note
&gt;&gt; Much more detailed information for transaction records is available
&gt;&gt; from a mirror node, and the mirror node retains transaction records
&gt;&gt; long term, rather than for a short &#34;cache&#34; duration. Clients may
&gt;&gt; prefer the mirror node graph API to query transaction records, rather
&gt;&gt; than this query.

&gt; Implementation Note
&gt;&gt; This query is _defined_ for &#34;Crypto&#34; service, but is _implemented_ by
&gt;&gt; the &#34;Network Admin&#34; service.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TransactionGetRecordQuery"></a>

### TransactionGetRecordQuery
Request for a `TransactionGetRecord` (a.k.a. `getTxRecordByTxID`) query.
&lt;p&gt;
A transaction record SHALL be available after the network reaches
consensus and completes execution for a transaction.&lt;br/&gt;
A transaction record SHALL NOT be available after the end of the network
configured &#34;record cache duration&#34;.

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
| includeDuplicates | [bool](#bool) |  | A flag to request duplicates. &lt;p&gt; If set, every transaction record within the record cache duration that matches the requested transaction identifier SHALL be returned.&lt;br/&gt; If not set, duplicate transactions SHALL NOT be returned.&lt;br/&gt; If not set, only the record for the first matching transaction to reach consensus SHALL be returned. |
| include_child_records | [bool](#bool) |  | A flag to request &#34;child&#34; records. &lt;p&gt; If set, the response SHALL include records for each child transaction executed as part of the requested parent transaction.&lt;br/&gt; If not set, the response SHALL NOT include any records for child transactions. |






<a name="proto-TransactionGetRecordResponse"></a>

### TransactionGetRecordResponse
Response message for a `getTxRecordByTxID` query.

The `transactionRecord` field SHALL return the record for the &#34;first&#34;
transaction that matches the transaction identifier requested.&lt;br/&gt;
If records for duplicate transactions are requested, those duplicate
records SHALL be present in the `duplicateTransactionReceipts` list.&lt;br/&gt;
If records for child transactions are requested, those child records SHALL
be present in the `child_transaction_records` list.&lt;br/&gt;
A state proof MAY be provided for this response; provided the record is
still available from the consensus nodes.

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
| transactionRecord | [TransactionRecord](#proto-TransactionRecord) |  | A transaction record. &lt;p&gt; This SHALL be the record for the &#34;first&#34; transaction that matches the transaction identifier requested.&lt;br/&gt; If the identified transaction has not reached consensus, this record SHALL have a `status` of `UNKNOWN`.&lt;br/&gt; If the identified transaction reached consensus prior to the current configured record cache duration, this record SHALL have a `status` of `UNKNOWN`. |
| duplicateTransactionRecords | [TransactionRecord](#proto-TransactionRecord) | repeated | A list of duplicate transaction records. &lt;p&gt; If the request set the `includeDuplicates` flat, this list SHALL include the records for each duplicate transaction associated to the requested transaction identifier. If the request did not set the `includeDuplicates` flag, this list SHALL be empty.&lt;br/&gt; If the `transactionRecord` status is `UNKNOWN`, this list SHALL be empty.&lt;br/&gt; This list SHALL be in order by consensus timestamp. |
| child_transaction_records | [TransactionRecord](#proto-TransactionRecord) | repeated | A list of records for all child transactions spawned by the requested transaction. &lt;p&gt; If the request set the `include_child_records` flag, this list SHALL include records for each child transaction executed as part of the requested parent transaction.&lt;br/&gt; If the request did not set the `include_child_records` flag, this list SHALL be empty. &lt;br/&gt; If the parent transaction did not initiate any child transactions this list SHALL be empty.&lt;br/&gt; If the `transactionRecord` status is `UNKNOWN`, this list SHALL be empty.&lt;br/&gt; This list SHALL be in order by consensus timestamp. |





 

 

 

 



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

