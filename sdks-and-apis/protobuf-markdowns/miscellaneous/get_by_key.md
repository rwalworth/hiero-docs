# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/get_by_key.proto](#services_get_by_key-proto)
    - [EntityID](#proto-EntityID)
    - [GetByKeyQuery](#proto-GetByKeyQuery)
    - [GetByKeyResponse](#proto-GetByKeyResponse)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_get_by_key-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/get_by_key.proto
# Get By Key
An obsolete query to obtain a list of entities that refer to
a given Key object.&lt;br/&gt;
Returned entities may be accounts, files, smart contracts, and/or
live hash entries.

&gt; Important
&gt;&gt; This query is obsolete and not supported.&lt;br/&gt;
&gt;&gt; Any query of this type that is submitted SHALL fail with a `PRE_CHECK`
&gt;&gt; result of `NOT_SUPPORTED`.

&gt; Implementation Note
&gt;&gt; This query is not defined for any service, and while it is implemented
&gt;&gt; in the &#34;Network Admin&#34; service, it may be unnecessary to do so.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-EntityID"></a>

### EntityID
The ID for a single entity (account, livehash, file, or smart contract)

&gt; The query that defines this message is no longer supported.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accountID | [AccountID](#proto-AccountID) |  | The Account ID for the cryptocurrency account |
| liveHash | [LiveHash](#proto-LiveHash) |  | A uniquely identifying livehash of an account |
| fileID | [FileID](#proto-FileID) |  | The file ID of the file |
| contractID | [ContractID](#proto-ContractID) |  | The smart contract ID that identifies instance |






<a name="proto-GetByKeyQuery"></a>

### GetByKeyQuery
Query all accounts, claims, files, and smart contract instances whose
associated keys include the given Key.

&gt; This query is no longer supported.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| key | [Key](#proto-Key) |  | The key to search for. It MUST NOT contain a contractID nor a ThresholdKey. |






<a name="proto-GetByKeyResponse"></a>

### GetByKeyResponse
Response when the client sends the node GetByKeyQuery

&gt; This query is no longer supported.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| entities | [EntityID](#proto-EntityID) | repeated | The list of entities that include this public key in their associated Key list |





 

 

 

 



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

