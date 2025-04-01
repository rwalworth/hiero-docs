# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_update_nfts.proto](#services_token_update_nfts-proto)
    - [TokenUpdateNftsTransactionBody](#proto-TokenUpdateNftsTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_update_nfts-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_update_nfts.proto
# Token Update NFTs
Given a token identifier and a metadata block, change the metadata for
one or more non-fungible/unique token instances.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenUpdateNftsTransactionBody"></a>

### TokenUpdateNftsTransactionBody
Modify the metadata field for an individual non-fungible/unique token (NFT).

Updating the metadata of an NFT SHALL NOT affect ownership or
the ability to transfer that NFT.&lt;br/&gt;
This transaction SHALL affect only the specific serial numbered tokens
identified.
This transaction SHALL modify individual token metadata.&lt;br/&gt;
This transaction MUST be signed by the token `metadata_key`.&lt;br/&gt;
The token `metadata_key` MUST be a valid `Key`.&lt;br/&gt;
The token `metadata_key` MUST NOT be an empty `KeyList`.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token | [TokenID](#proto-TokenID) |  | A token identifier.&lt;br/&gt; This is the token type (i.e. collection) for which to update NFTs. &lt;p&gt; This field is REQUIRED.&lt;br/&gt; The identified token MUST exist, MUST NOT be paused, MUST have the type non-fungible/unique, and MUST have a valid `metadata_key`. |
| serial_numbers | [int64](#int64) | repeated | A list of serial numbers to be updated. &lt;p&gt; This field is REQUIRED.&lt;br/&gt; This list MUST have at least one(1) entry.&lt;br/&gt; This list MUST NOT have more than ten(10) entries. |
| metadata | [google.protobuf.BytesValue](#google-protobuf-BytesValue) |  | A new value for the metadata. &lt;p&gt; If this field is not set, the metadata SHALL NOT change.&lt;br/&gt; This value, if set, MUST NOT exceed 100 bytes. |





 

 

 

 



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

