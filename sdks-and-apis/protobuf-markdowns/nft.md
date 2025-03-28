# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/token/nft.proto](#services_state_token_nft-proto)
    - [Nft](#proto-Nft)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_token_nft-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/token/nft.proto
# NFT.
This is a single, whole, unique, non-fungible token within the Hedera network.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-Nft"></a>

### Nft
An Hedera Token Service non-fungible token (NFT).&lt;br/&gt;
Every NFT is a unique instance of a token with non-fungible type.

The NFT SHALL be identified by token ID and serial number.&lt;br/&gt;
The token treasury account SHALL own all minted NFTs of that token type
initially.&lt;br/&gt;
NFTs owned by the token treasury SHALL NOT be linked into that account&#39;s
virtual linked list of NFTs.&lt;br/&gt;
NFTs not owned by the token treasury SHALL be linked into the owner
account&#39;s virtual linked list of NFTs.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| nft_id | [NftID](#proto-NftID) |  | The id of this NFT, consisting of a Token ID and serial number. |
| owner_id | [AccountID](#proto-AccountID) |  | The account or contract id that owns this NFT. &lt;p&gt; If this NFT is owned by its token type&#39;s current treasury account, this value SHALL be zero. |
| spender_id | [AccountID](#proto-AccountID) |  | The account or contract id approved to spend this NFT. &lt;p&gt; If there is no approved spender, this value SHALL be null. |
| mint_time | [Timestamp](#proto-Timestamp) |  | The consensus time of the TokenMint that created this NFT as offset from the epoch. &lt;p&gt; For this purpose, `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`. |
| metadata | [bytes](#bytes) |  | The metadata bytes for this NFT. This is often a URI value. &lt;p&gt; This value, if set, SHALL NOT exceed 100 bytes. |
| owner_previous_nft_id | [NftID](#proto-NftID) |  | The NFT ID of the previous entry in the current owner&#39;s &#34;virtual double-linked list&#34; of owned NFTs. &lt;p&gt; If the owner of this NFT is the token treasury, this SHALL be unset. |
| owner_next_nft_id | [NftID](#proto-NftID) |  | The NFT ID of the next entry in the current owner&#39;s &#34;virtual double-linked list&#34; of owned NFTs. &lt;p&gt; If the owner of this NFT is the token treasury, this SHALL be unset. |





 

 

 

 



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

