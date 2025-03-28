# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/crypto_get_stakers.proto](#services_crypto_get_stakers-proto)
    - [AllProxyStakers](#proto-AllProxyStakers)
    - [CryptoGetStakersQuery](#proto-CryptoGetStakersQuery)
    - [CryptoGetStakersResponse](#proto-CryptoGetStakersResponse)
    - [ProxyStaker](#proto-ProxyStaker)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_crypto_get_stakers-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/crypto_get_stakers.proto
# Get Stakers
Query all of the accounts proxy staking _to_ a specified account.

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


<a name="proto-AllProxyStakers"></a>

### AllProxyStakers
All of the accounts proxy staking to a given account, and the amounts proxy
staked


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accountID | [AccountID](#proto-AccountID) |  | The Account ID that is being proxy staked to |
| proxyStaker | [ProxyStaker](#proto-ProxyStaker) | repeated | Each of the proxy staking accounts, and the amount they are proxy staking |






<a name="proto-CryptoGetStakersQuery"></a>

### CryptoGetStakersQuery
Get all the accounts that are proxy staking to this account. For each of
them, give the amount currently staked. This was never implemented.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| accountID | [AccountID](#proto-AccountID) |  | The Account ID for which the records should be retrieved |






<a name="proto-CryptoGetStakersResponse"></a>

### CryptoGetStakersResponse
Response when the client sends the node CryptoGetStakersQuery


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| stakers | [AllProxyStakers](#proto-AllProxyStakers) |  | List of accounts proxy staking to this account, and the amount each is currently proxy staking |






<a name="proto-ProxyStaker"></a>

### ProxyStaker
information about a single account that is proxy staking


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accountID | [AccountID](#proto-AccountID) |  | The Account ID that is proxy staking |
| amount | [int64](#int64) |  | The number of hbars that are currently proxy staked |





 

 

 

 



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

