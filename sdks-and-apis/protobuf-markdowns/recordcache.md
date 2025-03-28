# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/recordcache/recordcache.proto](#services_state_recordcache_recordcache-proto)
    - [TransactionReceiptEntries](#proto-TransactionReceiptEntries)
    - [TransactionReceiptEntry](#proto-TransactionReceiptEntry)
    - [TransactionRecordEntry](#proto-TransactionRecordEntry)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_recordcache_recordcache-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/recordcache/recordcache.proto
# Record Cache
The Record Cache holds transaction records for a short time, and is the
source for responses to `transactionGetRecord` and `transactionGetReceipt`
queries.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TransactionReceiptEntries"></a>

### TransactionReceiptEntries
A cache of transaction receipts.&lt;br/&gt;
As transactions are handled and receipts are created, they are stored in
state for a configured time limit (perhaps, for example, 3 minutes).
During this time window, any client can query the node and get the receipt
for the transaction. The `TransactionReceiptEntries` is the object stored in
state with this information.

This message SHALL contain a list of `TransactionReceiptEntry` objects.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| entries | [TransactionReceiptEntry](#proto-TransactionReceiptEntry) | repeated |  |






<a name="proto-TransactionReceiptEntry"></a>

### TransactionReceiptEntry
An entry in the record cache with the receipt for a transaction.
This is the entry stored in state that enables returning the receipt
information when queried by clients.

When a transaction is handled a receipt SHALL be created.&lt;br/&gt;
This receipt MUST be stored in state for a configured time limit
(e.g. 3 minutes).&lt;br/&gt;
While a receipt is stored, a client MAY query the node and retrieve
the receipt.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | A node identifier.&lt;br/&gt; This identifies the node that submitted the transaction to consensus. The value is the identifier as known to the current address book. &lt;p&gt; Valid node identifiers SHALL be between 0 and &lt;tt&gt;2&lt;sup&gt;63-1&lt;/sup&gt;&lt;/tt&gt;, inclusive. |
| transaction_id | [TransactionID](#proto-TransactionID) |  | A transaction identifier.&lt;br/&gt; This identifies the submitted transaction for this receipt. |
| status | [ResponseCodeEnum](#proto-ResponseCodeEnum) |  | A status result.&lt;br/&gt; This is the final status after handling the transaction. |






<a name="proto-TransactionRecordEntry"></a>

### TransactionRecordEntry
As transactions are handled and records and receipts are created, they are
stored in state for a configured time period (for example, 3 minutes).
During this time, any client can query the node and get the record or receipt
for the transaction. The `TransactionRecordEntry` is the object stored in
state with this information.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [int64](#int64) |  | A node identifier.&lt;br/&gt; This identifier is the node, as known to the address book, that submitted the transaction for consensus. &lt;p&gt; This SHALL be a whole number. |
| payer_account_id | [AccountID](#proto-AccountID) |  | An Account identifier for the payer for the transaction. &lt;p&gt; This MAY be the same as the account ID within the Transaction ID of the record, or it MAY be the account ID of the node that submitted the transaction to consensus if the account ID in the Transaction ID was not able to pay. |
| transaction_record | [TransactionRecord](#proto-TransactionRecord) |  | A transaction record for the transaction. |





 

 

 

 



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

