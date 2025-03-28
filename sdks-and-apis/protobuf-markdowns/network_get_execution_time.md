# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/network_get_execution_time.proto](#services_network_get_execution_time-proto)
    - [NetworkGetExecutionTimeQuery](#proto-NetworkGetExecutionTimeQuery)
    - [NetworkGetExecutionTimeResponse](#proto-NetworkGetExecutionTimeResponse)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_network_get_execution_time-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/network_get_execution_time.proto
# Get Execution Time
Given a list of transaction identifiers, return the time required to
process each transaction, excluding pre-consensus processing, consensus,
and post-processing (e.g. record stream generation).

&gt; Important
&gt;&gt; This query is obsolete and not supported.&lt;br/&gt;
&gt;&gt; Any query of this type that is submitted SHALL fail with a `PRE_CHECK`
&gt;&gt; result of `NOT_SUPPORTED`.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-NetworkGetExecutionTimeQuery"></a>

### NetworkGetExecutionTimeQuery
Retrieve the time, in nanoseconds, spent in direct processing for one
or more recent transactions.

For each transaction identifier provided, if that transaction is
sufficiently recent (that is, it is within the range of the configuration
value `stats.executionTimesToTrack`), the node SHALL return the time, in
nanoseconds, spent to directly process that transaction.&lt;br/&gt;
This time will generally correspond to the time spent in a `handle` call
within the workflow.

Note that because each node processes every transaction for the Hedera
network, this query MAY be sent to any node, and results MAY be different
between different nodes.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| transaction_ids | [TransactionID](#proto-TransactionID) | repeated | A list of transaction identifiers to query. &lt;p&gt; All of the queried transaction identifiers MUST have execution time available. If any identifier does not have available execution time, the query SHALL fail with an `INVALID_TRANSACTION_ID` response. |






<a name="proto-NetworkGetExecutionTimeResponse"></a>

### NetworkGetExecutionTimeResponse
A response to a `networkGetExecutionTime` query.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| execution_times | [uint64](#uint64) | repeated | A list of execution times, in nanoseconds. &lt;p&gt; This list SHALL be in the same order as the transaction identifiers were presented in the query. |





 

 

 

 



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

