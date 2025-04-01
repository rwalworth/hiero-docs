# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/crypto_add_live_hash.proto](#services_crypto_add_live_hash-proto)
    - [CryptoAddLiveHashTransactionBody](#proto-CryptoAddLiveHashTransactionBody)
    - [LiveHash](#proto-LiveHash)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_crypto_add_live_hash-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/crypto_add_live_hash.proto
# Add Live Hash
Associate content to an account via a SHA-384 hash.

&gt; Important
&gt;&gt; This transaction is obsolete and not supported.&lt;br/&gt;
&gt;&gt; Any transaction of this type that is submitted SHALL fail
&gt;&gt; with a `PRE_CHECK` result of `NOT_SUPPORTED`.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-CryptoAddLiveHashTransactionBody"></a>

### CryptoAddLiveHashTransactionBody
Add a hash value to the ledger and associate it with an account.

Create an entry in the ledger for a SHA-384 hash of some content, and
associate that with a specific account.  This is sometimes used to associate
a credential or certificate with an account as a public record.&lt;br/&gt;
The entry created is also associated with a list of keys, all of which
MUST sign this transaction.&lt;br/&gt;
The account key for the associated account MUST sign this transaction.&lt;br/&gt;

The live hash, once created, MAY be removed from the ledger with one
or more signatures.
- The account key of the account associated to the live hash.
- Any one key from the key list in the live hash entry.
- Any combination of keys from the key list in the live hash entry.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| liveHash | [LiveHash](#proto-LiveHash) |  | A Live Hash to be added to the ledger and associated with the identified account. |






<a name="proto-LiveHash"></a>

### LiveHash
A Live Hash value associating some item of content to an account.

This message represents a desired entry in the ledger for a SHA-384
hash of some content, an associated specific account, a list of authorized
keys, and a duration the live hash is &#34;valid&#34;.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| accountId | [AccountID](#proto-AccountID) |  | An account associated via this live hash to the hashed content. |
| hash | [bytes](#bytes) |  | A SHA-384 hash of some content that is associated to the account or account holder. |
| keys | [KeyList](#proto-KeyList) |  | A list of keys, all of which MUST sign the transaction to add the live hash.&lt;br/&gt; Any one of these keys may, however, remove the live hash to revoke the association. |
| duration | [Duration](#proto-Duration) |  | A duration describing how long this Live Hash SHALL remain valid.&lt;br/&gt; A Live Hash SHOULD NOT be relied upon after this duration has elapsed. |





 

 

 

 



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

