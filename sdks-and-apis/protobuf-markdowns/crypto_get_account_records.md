# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/crypto_get_account_records.proto](#services_crypto_get_account_records-proto)
    - [CryptoGetAccountRecordsQuery](#proto-CryptoGetAccountRecordsQuery)
    - [CryptoGetAccountRecordsResponse](#proto-CryptoGetAccountRecordsResponse)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_crypto_get_account_records-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/crypto_get_account_records.proto
# Crypto Get Account Records
Messages for a query to retrieve recent transaction records involving a
specified account as effective `payer`.&lt;br/&gt;
A &#34;recent&#34; transaction is typically one that reached consensus within
the previous three(`3`) minutes of _consensus_ time. Additionally, the
network only stores records in state when
`ledger.keepRecordsInState=true` was true during transaction handling.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-CryptoGetAccountRecordsQuery"></a>

### CryptoGetAccountRecordsQuery
Request records of all &#34;recent&#34; transactions for which the specified
account is the effective payer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| accountID | [AccountID](#proto-AccountID) |  | An account identifier.&lt;br/&gt; This identifies the account to use when filtering the transaction record lists. &lt;p&gt; This field is REQUIRED. |






<a name="proto-CryptoGetAccountRecordsResponse"></a>

### CryptoGetAccountRecordsResponse
Return records of all &#34;recent&#34; transactions for which the specified
account is the effective payer.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| accountID | [AccountID](#proto-AccountID) |  | An account identifier.&lt;br/&gt; This identifies the account used when filtering the transaction record lists. &lt;p&gt; This field SHALL match the requested account identifier. |
| records | [TransactionRecord](#proto-TransactionRecord) | repeated | A list of records. &lt;p&gt; This list SHALL contain all available and &#34;recent&#34; records in which the account identified in the `accountID` field acted as effective payer. |





 

 

 

 



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

