# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/file_get_info.proto](#services_file_get_info-proto)
    - [FileGetInfoQuery](#proto-FileGetInfoQuery)
    - [FileGetInfoResponse](#proto-FileGetInfoResponse)
    - [FileGetInfoResponse.FileInfo](#proto-FileGetInfoResponse-FileInfo)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_file_get_info-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/file_get_info.proto
# File Get Information
Messages for a query to retrieve the metadata for a file in the
Hedera File Service (HFS).

The query defined here does not include the content of the file.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-FileGetInfoQuery"></a>

### FileGetInfoQuery
Query to request file metadata from the Hedera File Service (HFS).&lt;br/&gt;
This query requests all of the information _about_ a file, but none of the
_content_ of a file. A client should submit a `fileGetContents` query to
view the content of a file. File content _may_ also be available from a
block node or mirror node, generally at lower cost.

File metadata SHALL be available for active files and deleted files.&lt;br/&gt;
The size of a deleted file SHALL be `0` and the content SHALL be empty.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| fileID | [FileID](#proto-FileID) |  | A file identifier. &lt;p&gt; This MUST be the identifier of a file that exists in HFS.&lt;br/&gt; This value SHALL identify the file to be queried. |






<a name="proto-FileGetInfoResponse"></a>

### FileGetInfoResponse
A response to a query for the metadata of a file in the HFS.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| fileInfo | [FileGetInfoResponse.FileInfo](#proto-FileGetInfoResponse-FileInfo) |  | A combination of fields from the requested file metadata. &lt;p&gt; This SHALL NOT be set if the identified file does not exist or has expired. |






<a name="proto-FileGetInfoResponse-FileInfo"></a>

### FileGetInfoResponse.FileInfo



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fileID | [FileID](#proto-FileID) |  | A file identifier. &lt;p&gt; This SHALL be the identifier of a file that exists in HFS.&lt;br/&gt; This value SHALL identify the file that was queried. |
| size | [int64](#int64) |  | A size, in bytes, for the file. |
| expirationTime | [Timestamp](#proto-Timestamp) |  | An expiration timestamp. &lt;p&gt; The file SHALL NOT expire before the network consensus time exceeds this value.&lt;br/&gt; The file SHALL expire after the network consensus time exceeds this value.&lt;br/&gt; |
| deleted | [bool](#bool) |  | A flag indicating this file is deleted. &lt;p&gt; A deleted file SHALL have a size `0` and empty content. |
| keys | [KeyList](#proto-KeyList) |  | A KeyList listing all keys that &#34;own&#34; the file. &lt;p&gt; All keys in this list MUST sign a transaction to append to the file content, or to modify file metadata.&lt;br/&gt; At least _one_ key in this list MUST sign a transaction to delete this file.&lt;br/&gt; If this is an empty `KeyList`, the file is immutable, cannot be modified or deleted, but MAY expire. A `fileUpdate` transaction MAY extend the expiration time for an immutable file. |
| memo | [string](#string) |  | A short description for this file. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| ledger_id | [bytes](#bytes) |  | A ledger identifier for the responding network. &lt;p&gt; This value SHALL identify the distributed ledger that responded to this query. |





 

 

 

 



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

