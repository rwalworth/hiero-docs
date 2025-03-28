# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/token/account_pending_airdrop.proto](#services_state_token_account_pending_airdrop-proto)
    - [AccountPendingAirdrop](#proto-AccountPendingAirdrop)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_token_account_pending_airdrop-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/token/account_pending_airdrop.proto
# Account Pending Airdrop.
A single pending airdrop awaiting claim by the recipient.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-AccountPendingAirdrop"></a>

### AccountPendingAirdrop
A node within a doubly linked list of pending airdrop references.&lt;br/&gt;
This internal state message forms the entries in a doubly-linked list
of references to pending airdrop entries that are &#34;owed&#34; by a particular
account as &#34;sender&#34;.

Each entry in this list MUST refer to an existing pending airdrop.&lt;br/&gt;
The pending airdrop MUST NOT be claimed.&lt;br/&gt;
The pending airdrop MUST NOT be canceled.&lt;br/&gt;
The pending airdrop `sender` account&#39;s `head_pending_airdrop_id` field
MUST match the `pending_airdrop_id` field in this message.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| pending_airdrop_value | [PendingAirdropValue](#proto-PendingAirdropValue) |  | An amount of fungible tokens to be sent for this pending airdrop. &lt;p&gt; This field SHALL NOT be set for non-fungible/unique tokens. |
| previous_airdrop | [PendingAirdropId](#proto-PendingAirdropId) |  | A pending airdrop identifier. &lt;p&gt; This field SHALL identify the specific pending airdrop that precedes this position within the doubly linked list of pending airdrops &#34;owed&#34; by the sending account associated with this account airdrop &#34;list&#34;.&lt;br/&gt; This SHALL match `pending_airdrop_id` if this is the only entry in the &#34;list&#34;. |
| next_airdrop | [PendingAirdropId](#proto-PendingAirdropId) |  | A pending airdrop identifier.&lt;br/&gt; &lt;p&gt; This field SHALL identify the specific pending airdrop that follows this position within the doubly linked list of pending airdrops &#34;owed&#34; by the sending account associated with this account airdrop &#34;list&#34;.&lt;br/&gt; This SHALL match `pending_airdrop_id` if this is the only entry in the &#34;list&#34;. |





 

 

 

 



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

